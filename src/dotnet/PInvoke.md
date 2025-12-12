# Platform Invoke (P/Invoke)


https://openjdk.org/projects/panama/
https://github.com/microsoft/ClangSharp
https://github.com/mono/CppSharp


- [../tool/clangsharp.md](../tool/clangsharp.md) 참고


- https://learn.microsoft.com/en-us/dotnet/standard/native-interop/native-library-loading

``` txt
DllImport 순서

[DllImport("nativedep")]
static extern int ExportedFunction();


# Windows
nativedep
nativedep.dll

# Linux
nativedep.so
libnativedep.so
nativedep
libnativedep

# macOS
nativedep.dylib
libnativedep.dylib
nativedep
libnativedep
```


- DllImportAttribute 
  - https://learn.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.dllimportattribute
- SetDllImportResolver
  - DllImport나 SetDllImportResolver를 통해 로드된 네이티브 라이브러리는 .NET 런타임(또는 Unity의 IL2CPP 런타임) 이 수명(lifetime)을 관리합니다. 해당 Assembly가 unload되거나 앱 종료 시 자동 해제됩니다.
  - https://learn.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.nativelibrary.setdllimportresolver?view=net-9.0
- LibraryImportAttribute 
  - https://learn.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.libraryimportattribute


https://github.com/netpyoung/SqlCipher4Unity3D/tree/master
https://github.com/netpyoung/unity.webp
https://github.com/netpyoung/unity.libsodium

https://github.com/netpyoung/prebuilt-libsqlcipher/blob/main/NOTE.md
https://github.com/netpyoung/prebuilt-libwebp/blob/main/NOTE.md


https://github.com/netpyoung/prebuilt-libsodium
https://github.com/netpyoung/prebuilt-libwebp
https://github.com/netpyoung/prebuilt-libsqlcipher




## Unity 한정

| 플랫폼      | 런타임이 찾는 실제 파일 | 비고                                |
| ----------- | ----------------------- | ----------------------------------- |
| **Windows** | `hello.dll`             |                                     |
| **Linux**   | `libhello.so`           |                                     |
| **macOS**   | `libhello.dylib`        |                                     |
| **iOS **    | 불가                    | 빌드 시 정적 링크 필요 `__Internal` |

- IL2CPP/Mono 런타임이 추가되면서 .dylib도 지원하게 되었지만, Unity 문서와 빌드 파이프라인에는 여전히 .bundle이 병행 표기

- https://endoflife.date/unity
- C#의 정적 생성자(static ClassName())는 명시적으로 클래스가 처음 “사용될 때” 한 번만 호출됩니다.

``` cs
[RuntimeInitializeOnLoadMethod(RuntimeInitializeLoadType.BeforeSceneLoad)]
static void Init()
{
    _ = typeof(CustomResolverDemo); // 강제로 로드
}

```

- [../tool/clangsharp.md](../tool/clangsharp.md)


--- 

https://learn.microsoft.com/en-us/dotnet/framework/interop/default-marshalling-for-strings
https://github.com/dotnet/runtime/blob/main/docs/design/features/source-generator-pinvokes.md
https://learn.microsoft.com/en-us/dotnet/standard/native-interop/type-marshalling


Win32  - https://github.com/microsoft/CsWin32



- bass.net with MAUI iOS cannot load native bass library
  - https://www.un4seen.com/forum/?topic=19928.0
