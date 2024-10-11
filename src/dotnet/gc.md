# gc

## Memory

|     |                    |         |             |
| --- | ------------------ | ------- | ----------- |
| SOH | Small Object Heap  | x < 85K |             |
| LOH | Large Object Heap  | 85k < x |             |
| POH | Pinned Object Heap | pinning | from .NET 5 |

- [LOH](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/large-object-heap)

## GC

Gabage Collection

- generation
  - 0세대: 모든 새로운 개체가 이 세대에 들어갑니다.
    - 수명이 짧은 개체의 예로는 임시 변수가 있습니다. 가비지 수집은 이 세대에서 가장 자주 발생합니다.
  - 1세대: 한 번의 가비지 수집에서 살아남은 0세대의 개체가 이 세대로 이동됩니다.
  - 2세대: 두 번째 가비지 수집 후에도 남아 있는 1세대 개체가 이 세대로 이동됩니다.
  - 3세대: 2세대의 일부로 논리적으로 수집되는 물리적 세대입니다.
- promotion
  - 0세대 가비지 수집 후에도 유지되는 개체는 1세대로 승격됩니다.
  - 1세대 가비지 수집 후에도 남아 있는 개체는 2세대로 승격됩니다.
  - 2세대 가비지 수집 후에도 유지되는 개체는 2세대에 남아 있습니다.




## 단편화 Fragmentation

- 외부 단편화
  - 여유 메모리가 여러 블록으로 분산되어, 새로운 메모리 할당시 충분한 공간이 없을때
- 내부 단편화
  - 필요한 메모리보다 더 많이 할당. 남는 메모리 아까워

## Unity

Unity는 BDW Boehm–Demers–Weiser
  Boehm GC는 수집할 관리 오브젝트를 찾아 모든 스레드 스택(관리되는 코드와 네이티브 코드 포함)을 스캔하며, 관리되는 오브젝트를 할당하고 나면 해당 오브젝트의 메모리 내 위치는 절대 변하지 않습니다
.NET은 CoreCLR GC
  CoreCLR GC는 관리되는 코드에서만 할당된 오브젝트를 추적하며, 메모리에서 오브젝트를 옮겨 성능을 향상합니다.


- https://docs.unity3d.com/Manual/performance-incremental-garbage-collection.html
- https://docs.unity3d.com/ScriptReference/Scripting.GarbageCollector.html

## Ref

- <https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/large-object-heap>
- https://hackernoon.com/memory-mastery-comparing-unity-and-net-garbage-collection
https://www.hboehm.info/gc/
https://en.wikipedia.org/wiki/Tracing_garbage_collection#Stop-the-world_vs._incremental_vs._concurrent
https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/fundamentals
