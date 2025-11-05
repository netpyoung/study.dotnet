# clangsharp

- <https://github.com/dotnet/ClangSharp>


CXError_Failure
dotnet tool install --global ClangSharpPInvokeGenerator

libClang: https://www.nuget.org/packages/libclang.runtime.osx-arm64
libClangSharp: https://www.nuget.org/packages/libClangSharp.runtime.osx-arm64


/opt/homebrew/opt/llvm/include/

include/clang-c/CXErrorCode.h CXError_Failure


##

``` txt
# cmd에서는
ClangSharpPInvokeGenerator @hello.rsp

# pwsh에서는
## 방법 A: & (call operator) + 문자열
& ClangSharpPInvokeGenerator "@hello.rsp"

## 방법 B: --% (stop parsing) 사용
ClangSharpPInvokeGenerator --% @hello.rsp
```

- 명령줄이 너무 길어질 때, 옵션을 파일에 넣고 @파일명으로 한 번에 로드하는 방식.
  - 명령줄 길이 제한 회피 (Windows는 8191자 제한)


|                     |                                                 |
| ------------------- | ----------------------------------------------- |
| --config            | 아래 따로                                       |
| --language          | c , c++                                         |
| --include-directory | include 폴더 지정                               |
| --file-directory    | 파일을 찾기 위한 base 폴더                      |
| --file              | 어떠한 파일인지                                 |
| --libraryPath       | DllImport 시 사용할 이름                        |
| --namespace         | 네임스페이스 지정                               |
| --output            | output 폴더                                     |
| --additional        | Clang에 직접 임의 인수 전달.                    |
| --methodClassName   | 메소드들이 담길 클래스(파일) 명. 기본값 Methods |

| --config                |     |
| ----------------------- | --- |
| compatible-codegen      |     |
| multi-file              |     |
| unix-types              |     |
| generate-macro-bindings |     |
| windows-types           |     |
| log-visited-files       |     |



``` txt
--config
compatible-codegen
generate-macro-bindings
multi-file
unix-types
log-visited-files
--language
c
--include-directory
bass
C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.44.35207\include
--file-directory 
bass
--file
bassmidi.h
--namespace
unity.libwebp.Interop
--libraryPath
bassmidi
--output
./InteropLibwebp_BASSMIDI
--additional
-U_WIN32
```

typedef DWORD (CALLBACK STREAMPROC)(HSTREAM handle, void *buffer, DWORD length, void *user);
#define STREAMPROC_DUMMY		(STREAMPROC*)0		// "dummy" stream

[NativeTypeName("#define STREAMPROC_DUMMY (STREAMPROC*)0")]
public static readonly IntPtr STREAMPROC_DUMMY = &(nint)(0);

``` cs
// https://github.com/dotnet/ClangSharp/blob/main/sources/ClangSharp.Interop/Internals/NativeTypeNameAttribute.cs

[Conditional("DEBUG")]
[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Parameter | AttributeTargets.ReturnValue, AllowMultiple = false, Inherited = true)]
internal sealed class NativeTypeNameAttribute(string name) : Attribute
{
    public string Name { get; } = name;
}



// dotnet 버전 영향있으니 복붙용
[Conditional("DEBUG")]
[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Parameter | AttributeTargets.ReturnValue, AllowMultiple = false, Inherited = true)]
internal sealed class NativeTypeNameAttribute : Attribute
{
    public NativeTypeNameAttribute(string name) { Name = name; }
    public string Name { get; }
}
```

- LibraryImport 지원 관련 이슈가 있었는데 지원 안하는 걸로
  - https://github.com/dotnet/ClangSharp/issues/427
  -  [../dotnet/PInvoke.md](../dotnet/PInvoke.md) 참고



Feature 'parameterless struct constructors' is not available in C# 9.0. Please use language version 10.0 or greater.
    public partial struct BASS_3DVECTOR
    {
        public BASS_3DVECTOR()
        {
        }
    }
## Ref

- <https://apple.stackexchange.com/questions/227026/how-to-install-recent-clang-with-homebrew>