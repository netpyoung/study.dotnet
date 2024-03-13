
## coroutine

https://docs.unity3d.com/Manual/Coroutines.html
StartCoroutine
https://github.com/Unity-Technologies/UnityCsReference/blob/e740821767d2290238ea7954457333f06e952bad/Runtime/Export/Scripting/MonoBehaviour.bindings.cs#L81


## Task

TPL
https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/task-parallel-library-tpl
Task-based asynchronous programming
https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/task-based-asynchronous-programming
    // Task.Run
    https://github.com/dotnet/runtime/blob/3175a8a037d18cfbc355e4368c2432d3b7d7e178/src/libraries/System.Private.CoreLib/src/System/Threading/Tasks/Task.cs#L5339
    // Task.Factory.StartNew
    https://github.com/dotnet/runtime/blob/3175a8a037d18cfbc355e4368c2432d3b7d7e178/src/libraries/System.Private.CoreLib/src/System/Threading/Tasks/TaskFactory.cs#L279
ValueTask
https://stackoverflow.com/questions/43000520/why-would-one-use-taskt-over-valuetaskt-in-c
https://devblogs.microsoft.com/dotnet/understanding-the-whys-whats-and-whens-of-valuetask/
Dissecting the async methods in C#
https://devblogs.microsoft.com/premier-developer/dissecting-the-async-methods-in-c/





## TaskScheduler
https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskscheduler?view=net-6.0


## SynchronizationContext 
C# SynchronizationContext 와 await
 - SynchronizationContext에 현재 쓰레드 정보를 저장해 놨다 다른 쓰레드에서 상호작용에서 필요시 꺼내 쓴다.
https://korsa.tistory.com/94


https://docs.microsoft.com/en-us/dotnet/api/system.threading.synchronizationcontext?view=net-6.0
  UnitySynchronizationContext
    https://github.com/Unity-Technologies/UnityCsReference/blob/master/Runtime/Export/Scripting/UnitySynchronizationContext.cs
  UniTaskSynchronizationContext
    https://github.com/Cysharp/UniTask/blob/master/src/UniTask/Assets/Plugins/UniTask/Runtime/UniTaskSynchronizationContext.cs


- [Parallel Computing - It's All About the SynchronizationContext](https://docs.microsoft.com/en-us/archive/msdn-magazine/2011/february/msdn-magazine-parallel-computing-it-s-all-about-the-synchronizationcontext)

The idea behind ISynchronizeInvoke is that a “source” thread can queue a delegate to a “target” thread, optionally waiting for that delegate to complete.
ASP.NET asynchronous pages aren’t associated with a single thread. Instead of queuing work to the original thread, asynchronous pages only need to maintain a count of outstanding operations to determine when the page request can be completed.
After much thought and careful design, ISynchronizeInvoke was replaced by SynchronizationContext.

provides a way to queue a unit of work to a context. - SynchronizationContext does not include a mechanism to determine if synchronization is necessary, because this isn’t always possible.
every thread has a “current” context.
 it keeps a count of outstanding asynchronous operations
 
ConfigureAwait provides a means to avoid the default SynchronizationContext capturing behavior; 
	passing false for the flowContext parameter prevents the SynchronizationContext from being used to resume execution after the await.
There’s also an extension method on SynchronizationContext instances called SwitchTo;
	this allows any async method to change to a different SynchronizationContext by invoking SwitchTo and awaiting the result.
	
	
	
## ConfigureAwait
https://devblogs.microsoft.com/dotnet/configureawait-faq/

https://www.sysnet.pe.kr/2/0/11418
반면, ConfigureAwait에 false를 설정하면 await 이후의 코드를 SynchronizationContext에 태우지 않고 Task.StartNew로 생성되었던 그 Task의 스레드를 이용해 실행하므로 결과가 다음과 같이 나옵니다. (콘솔 응용 프로그램에서의 동작과 같습니다.)
따라서, SynchronizationContext가 있는 상황에서 await 호출을 하는 경우,
 UI 객체를 건드리는 작업이 없다면 굳이 SynchronizationContext에 태울 필요가 없으므로 그런 경우에는 가능한 개발자가 ConfigureAwait(false)를 설정해 주는 것이 성능상 더 유리합니다.

https://blog.stephencleary.com/2012/07/dont-block-on-async-code.html


https://docs.microsoft.com/en-us/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming?WT.mc_id=DT-MVP-4038148


## async stream
https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-8.0/async-streams
https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/tutorials/generate-consume-asynchronous-stream
https://dev.to/noseratio/asynchronous-coroutines-with-c-8-0-and-iasyncenumerable-2e04

============



https://stackoverflow.com/a/55085418
the thread pool task scheduler and a synchronization context task scheduler
https://github.com/dotnet/runtime/blob/c3fa7655ea3006f010780d325addcd04aaffa76b/src/libraries/System.Private.CoreLib/src/System/Threading/Tasks/TaskScheduler.cs#L297
https://github.com/dotnet/runtime/blob/c3fa7655ea3006f010780d325addcd04aaffa76b/src/libraries/System.Private.CoreLib/src/System/Threading/Tasks/ThreadPoolTaskScheduler.cs#L21




# TAP

`T`ask-based `A`synchronous `P`attern


| Target Version | language version | Unity version |
| -------------- | ---------------- | ------------- |
| .NET 7.x       | C# 11            |               |
| .NET 6.x       | C# 10            |               |
| .NET 5.x       | C# 9.0           | 2021.2        |
| .NET Core 3.x  | C# 8.0           | 2020.2        |



|                    |                       |
| ------------------ | --------------------- |
| .NET Framework 1.1 | Thread                |
| .NET Framework 4.0 | Task.Factory.StartNew |
|                    |                       |


Thread
Coroutine
IEnumerator
SynchronizationContext
TaskFactory
Task
  Unwrap, ContinueWith

generated: IAsyncStateMachine
ValueTask
AsyncMethodBuilder
TaskScheduler
TaskCreationOptions
CancellationToken


## Task


- Unwrap
  - Creates a proxy Task


``` cs
public static System.Threading.Tasks.Task Unwrap (this System.Threading.Tasks.Task<System.Threading.Tasks.Task> task);
Task<Task<int>> barMarker = Bar();
Task<int> awaitedMarker = await barMarker;
Task<int> unwrappedMarker = barMarker.Unwrap();
```

- ContinueWith
  - Creates a continuation that executes asynchronously when the target Task completes.

CancellationTokenSource
cts.ThrowIfCancellationRequested(); => throw OperationCanceledException

- Task.Run하고 Task.Factory.StartNew
  - Task.Run (someAction);
    - Task.InternalStartNew
  - Task.Factory.StartNew(someAction, CancellationToken.None, TaskCreationOptions.DenyChildAttach, TaskScheduler.Default);
    - Task.InternalStartNew

TaskCreationOptions.DenyChildAttach

``` cs
public Task StartNew(Action action)
{
    Task? currTask = Task.InternalCurrent;
    return Task.InternalStartNew(
        currTask,
        action,
        null,
        m_defaultCancellationToken,
        GetDefaultScheduler(currTask),
        m_defaultCreationOptions,
        InternalTaskOptions.None);
}

internal static Task InternalStartNew(
        Task? creatingTask,
        Delegate action
        object? state,
        CancellationToken cancellationToken,
        TaskScheduler scheduler,
        TaskCreationOptions options,
        InternalTaskOptions internalOptions)
```

| Category                                            | Detached child tasks | Attached child tasks |
| --------------------------------------------------- | -------------------- | -------------------- |
| Parent waits for child tasks to complete.           | No                   | Yes                  |
| Parent propagates exceptions thrown by child tasks. | No                   | Yes                  |
| Status of parent depends on status of child.        | No                   | Yes                  |


## SynchronizationContext

- SynchronizationContext
  - thread동기화할때 사용하면 좋다


public static System.Threading.SynchronizationContext? Current { get; }

|                                    | SynchronizationContext.Current          |
| ---------------------------------- | --------------------------------------- |
| Unity MainThread  (UnityApiThread) | UnityEngine.UnitySynchronizationContext |
| Other Thread                       | null                                    |


  WPF - DispatcherSynchronizationContext
  MAUI - DispatcherQueueSynchronizationContext
ASP.NET  AspNetSynchronizationContext가 

Thread thread = Thread.CurrentThread;
int threadId = thread.ManagedThreadId;
int threadId = Thread.CurrentThread.ManagedThreadId;
int processorId = Thread.GetCurrentProcessorId();

SynchronizationContext context = SynchronizationContext.Current;

##
- TaskScheduler.FromCurrentSynchronizationContext
  - task 동기화할때 사용하면 좋다

QueueTask

TaskScheduler scheduler = TaskScheduler.FromCurrentSynchronizationContext();
TaskScheduler scheduler2 = TaskScheduler.Current;

namespace System.Threading.Tasks
{
    public abstract class TaskScheduler
    {
      public static TaskScheduler Current => InternalCurrent ?? Default;
      internal static TaskScheduler? InternalCurrent
      {
          get
          {
              Task? currentTask = Task.InternalCurrent;
          }
      }
      public static TaskScheduler FromCurrentSynchronizationContext()
      {
          return new SynchronizationContextTaskScheduler();
      }
    }
}


```

class Program
{
    static void Main()
    {
        ProcessThread thread = GetCurrentThread();
        Console.WriteLine($"현재 스레드의 ID: {thread.Id}");
        Console.WriteLine($"현재 스레드가 동작하는 코어: {GetProcessorAffinity(thread)}");
    }

    static ProcessThread GetCurrentThread()
    {
        int threadId = Thread.CurrentThread.ManagedThreadId;
        Process currentProcess = Process.GetCurrentProcess();
        return currentProcess.Threads.Cast<ProcessThread>().FirstOrDefault(t => t.Id == threadId);
    }

    static int GetProcessorAffinity(ProcessThread thread)
    {
        return (int)thread.ProcessorAffinity;
    }
}

```


| SynchronizationContext |       |
| ---------------------- | ----- |
| ctx.Send               | sync  |
| ctx.Post               | async |

### blabla can only be called from the main thread






Task 장/단점
ValueTask 장/단점


## ConfigureAwait

- https://devblogs.microsoft.com/dotnet/configureawait-faq/

``` cs
public System.Runtime.CompilerServices.ConfiguredTaskAwaitable ConfigureAwait (bool continueOnCapturedContext);
```

| continueOnCapturedContext | await 이후의 코드를 SynchronizationContext에 이용 |
| ------------------------- | ------------------------------------------------- |
| true - 기본값             | await 이후 현재 쓰레드에서 실행                   |
| false                     | await 이후 새로운 쓰레드에서 실행                     |




## Unity Coroutine
https://github.com/Unity-Technologies/UnityCsReference/blob/master/Runtime/Export/Scripting/Coroutine.bindings.cs

public Coroutine StartCoroutine(IEnumerator routine);
public void StopCoroutine(Coroutine routine);

IEnumerator

public abstract class CustomYieldInstruction : IEnumerator
public sealed class Coroutine : YieldInstruction


``` plantuml
Caller -> Coroutine: Call
Caller <- Coroutine: Yield
Caller -> Coroutine: Resume
Caller <- Coroutine: Yield

```

## UniTask

- https://github.com/Cysharp/UniTask

### UniTask 특징

- Struct based
- custom AsyncMethodBuilder


``` cs
// https://github.com/Cysharp/UniTask/blob/master/src/UniTask/Assets/Plugins/UniTask/Runtime/PlayerLoopHelper.cs
[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)]
static void Init()
{
    // capture default(unity) sync-context.
    unitySynchronizationContext = SynchronizationContext.Current;
    mainThreadId = Thread.CurrentThread.ManagedThreadId;
    ...
}
```
### UniTask 장/단점

- 테스트코드 유무


UniTaskVoid
Forget()

- Task<ParallelLoopResult> task = Task.Run(() => Parallel.ForEach(
- await Parallel.ForEachAsync
  - System.Threading.Tasks.Parallel.dll





## Ref

- https://learn.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap
- https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-10.0/async-method-builders
  - C#10
- https://devblogs.microsoft.com/pfxteam/task-run-vs-task-factory-startnew/
- https://www.sysnet.pe.kr/2/0/11417
- https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskcreationoptions?view=net-7.0
- Nested Tasks and Child Tasks
  - https://learn.microsoft.com/en-us/previous-versions/dotnet/netframework-4.0/dd997417(v=vs.100)
- UnityMainThreadDispatcher
  - https://github.com/PimDeWitte/UnityMainThreadDispatcher
- https://weblogs.asp.net/dixin/understanding-c-sharp-async-await-1-compilation
- https://www.sysnet.pe.kr/2/0/13191