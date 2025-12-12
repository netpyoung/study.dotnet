# Thread & Task

## coroutine

<https://docs.unity3d.com/Manual/Coroutines.html>

StartCoroutine
https://github.com/Unity-Technologies/UnityCsReference/blob/e740821767d2290238ea7954457333f06e952bad/Runtime/Export/Scripting/MonoBehaviour.bindings.cs#L81


``` cs
Task.Factory.StartNew(action)
	Task.Factory.StartNew(action, CancellationToken.None, TaskCreationOptions.None, TaskScheduler.Current);

Task.Run(action)
	Task.Factory.StartNew(action, CancellationToken.None, TaskCreationOptions.DenyChildAttach, TaskScheduler.Default);


TaskCreationOptions.None            TaskScheduler.Current
TaskCreationOptions.DenyChildAttach TaskScheduler.Default
```


## Task

- TPL (`T`ask `P`arallel `L`ibrary)

- <https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/task-parallel-library-tpl>
- Task-based asynchronous programming
- <https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/task-based-asynchronous-programming>
  - [Task.Run](https://github.com/dotnet/runtime/blob/3175a8a037d18cfbc355e4368c2432d3b7d7e178/src/libraries/System.Private.CoreLib/src/System/Threading/Tasks/Task.cs#L5339)
  - [Task.Factory.StartNew](https://github.com/dotnet/runtime/blob/3175a8a037d18cfbc355e4368c2432d3b7d7e178/src/libraries/System.Private.CoreLib/src/System/Threading/Tasks/TaskFactory.cs#L279)
- ValueTask
  - <https://stackoverflow.com/questions/43000520/why-would-one-use-taskt-over-valuetaskt-in-c>
  - <https://devblogs.microsoft.com/dotnet/understanding-the-whys-whats-and-whens-of-valuetask/>
- Dissecting the async methods in C#
  - <https://devblogs.microsoft.com/premier-developer/dissecting-the-async-methods-in-c/>
- Unwrap()의 목적:
  - [How to: Unwrap a Nested Task](https://learn.microsoft.com/en-us/dotnet/standard/parallel-programming/how-to-unwrap-a-nested-task)
  - `Task<Task<T>>를 Task<T>로 변환함.`
  - 중첩된 Task 구조를 단일 Task로 평탄화함

## TaskScheduler

- <https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskscheduler?view=net-6.0>


## SynchronizationContext 

- C# SynchronizationContext 와 await
  - SynchronizationContext에 현재 쓰레드 정보를 저장해 놨다 다른 쓰레드에서 상호작용에서 필요시 꺼내 쓴다.
  - https://korsa.tistory.com/94


- <https://docs.microsoft.com/en-us/dotnet/api/system.threading.synchronizationcontext?view=net-6.0>
  - [UnitySynchronizationContext](https://github.com/Unity-Technologies/UnityCsReference/blob/master/Runtime/Export/Scripting/UnitySynchronizationContext.cs)
  - [UniTaskSynchronizationContext](https://github.com/Cysharp/UniTask/blob/master/src/UniTask/Assets/Plugins/UniTask/Runtime/UniTaskSynchronizationContext.cs)

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

- <https://devblogs.microsoft.com/dotnet/configureawait-faq/>

- <https://www.sysnet.pe.kr/2/0/11418>


- 기본값은 ConfigureAwait(continueOnCapturedContext: true)입니다
  - true: await가 완료되도 SynchronizationContext 유지
  - false: await가 완료되도 SynchronizationContext 유지하지 않음.

- <https://blog.stephencleary.com/2012/07/dont-block-on-async-code.html>
- <https://docs.microsoft.com/en-us/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming?WT.mc_id=DT-MVP-4038148>


## async stream

- <https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-8.0/async-streams>
- <https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/tutorials/generate-consume-asynchronous-stream>
- <https://dev.to/noseratio/asynchronous-coroutines-with-c-8-0-and-iasyncenumerable-2e04>


- <https://stackoverflow.com/a/55085418>
the thread pool task scheduler and a synchronization context task scheduler
- <https://github.com/dotnet/runtime/blob/c3fa7655ea3006f010780d325addcd04aaffa76b/src/libraries/System.Private.CoreLib/src/System/Threading/Tasks/TaskScheduler.cs#L297>
- <https://github.com/dotnet/runtime/blob/c3fa7655ea3006f010780d325addcd04aaffa76b/src/libraries/System.Private.CoreLib/src/System/Threading/Tasks/ThreadPoolTaskScheduler.cs#L21>




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
| .NET Framework 4.0 | Task.Factory.StartNew |
| .NET Framework 1.1 | Thread                |


TaskFactory
Task
  Unwrap, ContinueWith

generated: IAsyncStateMachine
AsyncMethodBuilder
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

- Task.Run하고 Task.Factory.StartNew
  - Task.Run (someAction);
    - Task.InternalStartNew
  - Task.Factory.StartNew(someAction, CancellationToken.None, TaskCreationOptions.DenyChildAttach, TaskScheduler.Default);
    - Task.InternalStartNew

TaskCreationOptions.DenyChildAttach


| TaskCreationOptions (Flag)     | 설명                                                                                                                                        |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| None                           | 기본 동작을 사용함.특별한 요구사항이 없는 일반적인 경우                                                                                 |
| PreferFairness                 | 작업이 큐에 추가된 순서대로 실행되도록 함.	작업의 실행 순서가 중요한 경우                                                               |
| LongRunning                    | 장기 실행 작업임을 나타냅니다.	오래 실행되는 작업(예: 파일 I/O, 네트워크 작업)을 처리할 때                                                  |
| AttachedToParent               | 작업을 부모 작업에 연결함.	부모 작업이 완료되기 전에 자식 작업이 완료되어야 하는 경우                                                   |
| DenyChildAttach                | 자식 작업이 부모에 연결되는 것을 방지함.	독립적인 작업 실행이 필요한 경우                                                               |
| HideScheduler                  | 작업 내에서 생성된 새 작업이 현재 작업의 스케줄러를 상속받지 않고 기본 스케줄러를 사용해야 할 때 유용                                       |
| RunContinuationsAsynchronously | 연속 작업(continuation)이 현재 스레드를 차단하지 않고 비동기적으로 실행되어야 할 때 사용함. 데드락을 방지하는 데 도움이 될 수 있음. |

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

## CancellationTokenSource & CancellationToken

- CancellationTokenSource : IDisposable
  - Cancel()을 호출하여 취소 신호를 보내는 역할
    - Cancel을 여러 번 호출이 안전하며, 상태가 변경되지 않은 상태로 남아 있음.
  - Dispose전 Cancel도 해주자
  - CancellationTokenSource.Token => CancellationToken
- CancellationToken:
  - 실제로 취소가 발생했는지를 감지할 수 있는 수동적인 토큰.
  - 콜백이 이미 실행 중이거나 완료된 경우 다시 실행되지 않음.
  - IsCancellationRequested: 현재 취소 요청 여부를 확인할 수 있음.
  - ThrowIfCancellationRequested: OperationCanceledException 예외 발생

- <https://learn.microsoft.com/ko-kr/dotnet/api/system.threading.cancellationtoken?view=net-9.0>

``` cs
// 토큰은 합칠 수 도 있다.
CancellationToken combinedToken = CancellationTokenSource.CreateLinkedTokenSource(token1, token2).Token;
```

## SynchronizationContext

- SynchronizationContext
  - thread동기화할때 사용하면 좋다


public static System.Threading.SynchronizationContext? Current { get; }

|                                    | SynchronizationContext.Current          |
| ---------------------------------- | --------------------------------------- |
| Unity MainThread  (UnityApiThread) | UnityEngine.UnitySynchronizationContext |
| Other Thread                       | null                                    |

- ex
  - WPF - DispatcherSynchronizationContext
  - MAUI - DispatcherQueueSynchronizationContext
  - ASP.NET  AspNetSynchronizationContext

``` cs
Thread thread = Thread.CurrentThread;
int threadId = thread.ManagedThreadId;
int threadId = Thread.CurrentThread.ManagedThreadId;
int processorId = Thread.GetCurrentProcessorId();

SynchronizationContext context = SynchronizationContext.Current;
```

## TaskScheduler

- TaskScheduler.FromCurrentSynchronizationContext
  - UI쓰레드등. task 동기화할때 사용하면 좋다
  - 메인쓰레드(유니티)

| TaskScheduler                        | 설명                                                                                               |
| ------------------------------------ | -------------------------------------------------------------------------------------------------- |
| Default                              | 기본 스케줄러로, 스레드 풀을 사용함.	일반적인 비동기 작업 실행 시                              |
| Current                              | 현재 실행 중인 작업의 스케줄러를 반환함.	현재 컨텍스트에서 작업을 계속 실행해야 할 때          |
| FromCurrentSynchronizationContext    | 현재 동기화 컨텍스트에서 작업을 실행함. UI 스레드에서 작업을 실행해야 할 때 (WPF, WinForms 등) |
| SynchronizationContext               | Current 현재 동기화 컨텍스트를 사용함. UI스레드와의동기화가 필요한 경우                        |
| ConcurrentExclusiveSchedulerPair     | 동시성과 배타성을 제어하는 스케줄러 쌍을 생성함.	리소스 접근을 세밀하게 제어해야 할 때         |
| LimitedConcurrencyLevelTaskScheduler | 동시 실행 작업 수를 제한함.리소스 사용을 제한하거나 동시성을 제어해야 할 때                    |

``` cs
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

``` cs
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

## ConfigureAwait

- <https://devblogs.microsoft.com/dotnet/configureawait-faq/>

``` cs
// System.Runtime.CompilerServices

public ConfiguredTaskAwaitable ConfigureAwait (bool continueOnCapturedContext);
```

| continueOnCapturedContext | await 이후의 코드를 SynchronizationContext에 이용 |
| ------------------------- | ------------------------------------------------- |
| true - 기본값             | await 이후 현재 쓰레드에서 실행                   |
| false                     | await 이후 새로운 쓰레드에서 실행                 |


## Awaiter

``` cs
public TaskAwaiter<bool> GetAwaiter()
{
    return _downloadAllTask.GetAwaiter();
}

public bool IsCompleted
{
    get
    {
    }
}

public void GetResult() { }
```

### INotifyCompletion, ICriticalNotifyCompletion

``` cs
// INotifyCompletion
void OnCompleted(Action continuation)
{
}
```

``` cs
// ICriticalNotifyCompletion : System.Runtime.CompilerServices.INotifyCompletion

public void OnCompleted (Action continuation)
{
}
public void UnsafeOnCompleted (Action continuation)
{
}
```



## ValueTask

``` cs
public async Task<bool> MoveNextAsync()
{
    if (_bufferdCount == 0)
    {
        await FillBuffer();    // 새로운 Task<bool> 객체를 할당
    }
    return _bufferedCount > 0; // (true와 false), 오직 2개의 Task<bool> 객체만 필요.
}
```

``` cs
private readonly Dictionary<int, string> _cache = new Dictionary<int, string>();
public async ValueTask<string> GetCachedDataAsync(int id)
{
    if (_cache.TryGetValue(id, out string cachedData))
    {
        // 동기적으로 바로 반환 가능할 때, ValueTask가 메모리 할당을 줄여줌
        return cachedData;
    }

    // 없으면, 비동기 작업으로 데이터를 가져옴
    string data = await FetchDataFromDatabaseAsync(id);
    _cache[id] = data;
    return data;
}

private async Task<string> FetchDataFromDatabaseAsync(int id)
{
    await Task.Delay(100); // 데이터베이스에서 가져온다고 가정
    return $"Data_{id}";
}
```

- Task는 Class.
  - `Task<int>`같은경우 경우의 수가 많기에, 보다 적은수의 캐싱을 유지
  - 완료될 때까지 대기할 수 있으며, 실행이 완료되면 IsCompleted, Result, GetAwaiter().GetResult()와 같은 API가 작업을 완료 상태로 유지.
  - 여러 스레드에서 안전하게 접근할 수 있도록 설계됨.
- ValueTask는 Struct
  - 캐싱 걱정 없음
  - 기본적으로 싱글 컨슈머(single-consumer) 패턴을 전제로 하여 가볍게 설계됨.
  - ValueTask는 IValueTaskSource라는 인터페이스를 사용해 동작할 수 있으며, 메모리 할당을 최소화하는 방식으로 작동.
    - 이로 인해 ValueTask는 여러 스레드에서 동시에 접근하거나 두 번 이상 await하는 경우 문제가 발생할 수 있다.


| 상황                               | Task / Task\<TResult>                                                                                    | ValueTask / ValueTask\<TResult>                                                                                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 여러 번 `await`할 때               | 완료된 후에도 다시 불완료 상태로 전환되지 않으므로 여러 번 `await`해도 항상 동일한 결과를 얻을 수 있음. | 객체가 이미 다른 작업에서 재사용되고 있을 수 있어, 여러 번 `await`할 경우 문제가 발생할 수 있음.                                                                                |
| 동시에 `await`할 때                | 안정적으로 사용할 수 있음.                                                                              | 하나의 콜백만을 하나의 소비자와 함께 사용하도록 설계되었으므로, 동시 `await` 시 경쟁 상태와 같은 오류가 발생할 수 있음.                                                         |
| `GetAwaiter().GetResult()` 호출 시 | 작업이 완료될 때까지 호출자를 차단, 작업 완료 전에도 안전하게 사용할 수 있음.                           | 작업이 완료되지 않았을 때 호출하면 대기하지 않을 수 있어, 경쟁 상태가 발생하고 의도한 대로 동작하지 않을 가능성이 큽니다. `IValueTaskSource` 구현체는 지원을 보장하지 않음. |



- 메모리 효율성: 데이터가 캐시에 있을 경우, 동기적으로 완료되므로 Task 객체를 생성하지 않아 메모리 할당이 줄어듬.
- 메모리 할당 감소 효과: ValueTask는 주로 결과가 이미 완료된 작업이나 반복적인, 짧은 작업에서 메모리 할당을 줄이기 위해 사용됨.
- 복잡한 에러 처리 및 호출 방식: ValueTask를 사용할 때는 한 번만 await 가능하거나 단일 소비자 패턴을 따라야 하는 제약 사항이 있는데, 이 경우 ComputeFromStreamAsync를 호출할 코드가 많아질 수 있음. Task는 안전하게 여러 번 await할 수 있어 더 관리가 용이함.
- 동기 완료 가능성: ValueTask는 동기적으로 완료될 가능성이 높은 경우에 적합함. 이 메서드는 비동기적으로 스트림을 읽는 작업이므로 동기 완료 가능성이 거의 없어, ValueTask가 적절하지 않음.


## Parallel

- `Task<ParallelLoopResult> task = Task.Run(() => Parallel.ForEach(`
- await Parallel.ForEachAsync
  - System.Threading.Tasks.Parallel.dll

## TLS

``` cs
[ThreadStatic] 

ThreadLocal<T>

AsyncLocal<T>

ExecutionContext.SuppressFlow()
```


## Ref

- <https://learn.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap>
- <https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-10.0/async-method-builders>
  - C#10
- <https://devblogs.microsoft.com/pfxteam/task-run-vs-task-factory-startnew/>
- <https://www.sysnet.pe.kr/2/0/11417>
- <https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.taskcreationoptions?view=net-7.0>
- Nested Tasks and Child Tasks
  - <https://learn.microsoft.com/en-us/previous-versions/dotnet/netframework-4.0/dd997417(v=vs.100)>
- UnityMainThreadDispatcher
  - <https://github.com/PimDeWitte/UnityMainThreadDispatcher>
- <https://weblogs.asp.net/dixin/understanding-c-sharp-async-await-1-compilation>
- <https://www.sysnet.pe.kr/2/0/13191>