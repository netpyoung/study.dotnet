
$ git reset --hard
$ git rm --cached -r .
$ git add --renormalize .




visual studio Ctrl + K + D 
https://github.com/editorconfig/
https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/code-style-rule-options
https://github.com/dotnet/roslyn/blob/main/.editorconfig
https://github.com/dotnet/format
https://github.com/alirezanet/husky.net
https://github.com/versionize/versionize
https://www.conventionalcommits.org/en/v1.0.0/

https://semver.org/
1.0.0-dev+1
```
{major}.{minor}.{patch}-{stage}+{buildnumber}
```


https://techblog.kayac.com/unity-debug-with-slack


- No warning.

|                 |          |
| --------------- | -------- |
| member variable | _hello   |
| member function | _Hello() |
| struct          | SHello   |
| readonly struct | Hello    |
| interface       | IHello   |
| abstract        | AHello   |
| const           | HELLO    |
| static          | S_HELLO  |
| volatile        | v_hello  |
| enum            | E_HELLO  |
| bool variable   | isHello  |
| bool function   | IsHello  |
| float           | 0.5f     |

|                         |          |
| ----------------------- | -------- |
| Client->Server reqest   | QQ_Hello |
| Server->Client response | RR_Hello |
| Server sent             | SS_Hello |
| DataSheet (from exel)   | DS_Hello |
| Database                | DB_Hello |

Volatile > Static

``` cs
// using namespace
namespace System.Graphics
{
}

// ## class
// use sealed
// use { get; }
// use constructor with Named arguments
public sealed class Hello {}

// ## class - partial
// - OriginName.SubName.cs
public partial class Human; // // Human.Head.cs, Human.Head.cs

// ## enum
// - E_UPPER for enum
public enum E_HELLO
{
    HELLO = 0,
    WORLD = 1,
}

// ## null
// OrNull for nullable
public string GetNameOrNull(string nameOrNull)
{
    AssertNotNull(nameOrNull);

    return nameOrNull;
}

// ## Etc
// - acronyms. extra word
public int OrderID { get; private set; }  // ID -no extra word
public int HttpCode { get; private set; } // HTTP - extra word

// - recursive
public void FibonacciRecursive();

// ### avoid override
// - yes
public Anim GetAnimByIndex(int index);
public Anim GetAnimByName(string name);
// - no 
public Anim GetAnim(int index);
public Anim GetAnim(string name);


// ## default
// When default parameters are used, restrict them to natural immutable constants such as null, false or 0.

// no var
// if change typename, It is easy to notice which file impacted.
```

정상적으로 동작하는걸 건드리는건 리스크가 큼. 필요할 경우에만, 리뷰꼼꼼히.
스타일 코드리뷰는 빡세게 하는건 자제. 실수를 유발하는 스타일이 아니면, 스타일보다는 로직이 중요.

## Ref

- https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions
- https://github.com/netpyoung/vs.pocu.COMP3200/blob/master/code_style.md
- https://github.com/netpyoung/study.cpp/blob/master/coding_standard.md
- [코딩스탠다드: 함수 작성](https://www.youtube.com/watch?v=rbWSTXBYNFA)
- [POCU 아카데미용 C# 코딩 표준](https://docs.popekim.com/ko/coding-standards/pocu-csharp)
  - [eng](https://docs.popekim.com/en/coding-standards/csharp)