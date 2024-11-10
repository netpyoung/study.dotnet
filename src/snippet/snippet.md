
- https://linkdotnet.github.io/tips-and-tricks/

❌ **Bad**
✅ **Good**



async키워드를 생략하면 전체 상태 머신도 생략됩니다

https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.-ctor?view=net-8.0#system-collections-generic-list-1-ctor(system-int32)


List<string> dinosaurs = new List<string>(capacity: 4);



유니티
UnityWebRequest
op.webReqeust.Abort()

- 기본값은 ConfigureAwait(continueOnCapturedContext: true)입니다
  - true: await가 완료되도 SynchronizationContext 유지
  - false: await가 완료되도 SynchronizationContext 유지하지 않음.

CancelationTokenSource Cancel



StringComparer.OrdinalIgnoreCase
  public HashSet(int capacity, IEqualityComparer<T> comparer);
  new Dictionary<string, int>(StringComparer.OrdinalIgnoreCase)
  string.Equals("ABC", "abc", StringComparison.OrdinalIgnoreCase);
