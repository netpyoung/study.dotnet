# Source Generator

- 새 코드를 추가 가능.
- 기존 코드 수정 불가능
- C# 혹은 추가 파일에 접근 가능

## Unity

- 버전
  - .NET 버전
  - IDE의 컴파일러 버전
  - Microsoft.CodeAnalysis.CSharp 버전
    - /Editor/Data/DotNetSdkRoslyn/Microsoft.CodeAnalysis.CSharp.dll 버전보다 높은 버전의 것을 참조하면 움직이지 않는다.

Roslyn - 4.3.1
Visual Studio 2022 - 17.3

.NET Compiler Platform SDK


 - etc: Improved Interpolated Strings가 C# 10.0


- 빌드된.dll
  - 의존성 관리가 힘들 수 도 있으니 하나의 dll로 만드는게 좋음.
  - label: RoslynAnalyzer
  - dll배치는 Runtime/ 쪽에 놔두는 경향이 있음

## ISourceGenerator & IIncrementalGenerator

- deprecated - ISourceGenerator: 전체 코드 베이스
- IIncrementalGenerator: 변경된 부분

| Microsoft.CodeAnalysis.CSharp |                       | Unity            |
| ----------------------------- | --------------------- | ---------------- |
| 3.x                           | ISourceGenerator      |                  |
| 4.x                           | IIncrementalGenerator | 2022.2~, 2023.1~ |

## Sample: SourceGenerator

``` cmd
mkdir SourceGenerator
cd SourceGenerator
dotnet new gitignore
dotnet new classlib -o SourceGeneratorGen
dotnet new console -o SourceGeneratorSrc
dotnet new sln
dotnet sln add SourceGeneratorGen
dotnet sln add SourceGeneratorSrc
```

### SourceGeneratorGen

- 우클릭 소스제너레이터 프로젝트
- Properties > Debug > Open debug launch profiles UI
- 보이는 프로파일 삭제
- 추가 클릭
  - Roslyn component 선택
  - 타겟 프로젝트에서 콘솔 어플리케이션 프로젝트 선택
  - UI 닫기
- 비주얼 스튜디오 재시작
- 재생 버튼 옆 디버그 프로파일 드롭다운에서 소스제너레이터 프로젝트 선택
- 디버거가 멈추는지 확인하기 위해 소스제너레이터에 브레이크 포인트를 설정
- 재생 클릭

``` xml
<!-- SourceGeneratorGen/SourceGeneratorGen.csproj-->

<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
        <ImplicitUsings>disable</ImplicitUsings>
        <Nullable>disable</Nullable>

        <LangVersion>11</LangVersion>

        <!-- IsRoslynComponent: true => | Properties > Debug > Launch > Roslyn Component -->
        <IsRoslynComponent>true</IsRoslynComponent>
        <AnalyzerLanguage>cs</AnalyzerLanguage>

        <EmitCompilerGeneratedFiles>true</EmitCompilerGeneratedFiles>
        <CompilerGeneratedFilesOutputPath>Generated</CompilerGeneratedFilesOutputPath>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.CodeAnalysis.CSharp" Version="4.3.1" />
    </ItemGroup>
</Project>
```

``` csharp
// SourceGeneratorGen/Generator.cs

using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp.Syntax;
using System.Collections.Generic;
using System.Linq;

namespace SourceGeneratorGen
{
    [Generator(LanguageNames.CSharp)]
    public sealed class Generator : IIncrementalGenerator
    {
        public void Initialize(IncrementalGeneratorInitializationContext context)
        {
            // === IncrementalValue[s]Provider<T>
            //context.CompilationProvider
            //context.AdditionalTextsProvider
            //context.AnalyzerConfigOptionsProvider
            //context.MetadataReferencesProvider
            //context.ParseOptionsProvider
            // === SyntaxValueProvider
            // context.SyntaxProvider
            // === Outputting values
            //context.RegisterSourceOutput               // 사용자 컴파일에 포함될 소스 파일과 진단을 생성할 수 있다
            //context.RegisterImplementationSourceOutput // RegisterSourceOutput랑 비슷. 단, 유저코드나 다른 변환기에 의해 실행되지 않음. 코드 분석에 영향을 주지 않음.
            //context.RegisterPostInitializationOutput   // 다른 변환이 실행되기 전에 컴파일에 포함됨

            context.RegisterPostInitializationOutput(Callback);

            IncrementalValuesProvider<GeneratorAttributeSyntaxContext> source = context.SyntaxProvider.ForAttributeWithMetadataName(
                fullyQualifiedMetadataName: "GeneratedNamespace.GenerateToStringAttribute",
                predicate: static (node, token) => true,
                transform: static (context, token) => context);

            context.RegisterSourceOutput(source, Emit);
        }

        private void Callback(IncrementalGeneratorPostInitializationContext context)
        {
            string code = """
using System;

namespace GeneratedNamespace
{
    [AttributeUsage(AttributeTargets.Class, AllowMultiple = false, Inherited = false)]
    internal sealed class GenerateToStringAttribute : Attribute
    {
    }
}
""";
            context.AddSource("Generated.cs", code);
        }

        private void Emit(SourceProductionContext context, GeneratorAttributeSyntaxContext source)
        {
            INamedTypeSymbol typeSymbol = (INamedTypeSymbol)source.TargetSymbol;
            TypeDeclarationSyntax typeNode = (TypeDeclarationSyntax)source.TargetNode;

            if (typeSymbol.GetMembers("ToString").Length != 0)
            {
                context.ReportDiagnostic(Diagnostic.Create(DiagnosticDescriptors.ExistsOverrideToString, typeNode.Identifier.GetLocation(), typeSymbol.Name));
                return;
            }

            string ns;
            if (typeSymbol.ContainingNamespace.IsGlobalNamespace)
            {
                ns = string.Empty;
            }
            else
            {
                ns = $"{typeSymbol.ContainingNamespace}";
            }

            string fullType = typeSymbol
                .ToDisplayString(SymbolDisplayFormat.FullyQualifiedFormat)
                .Replace("global::", "")
                .Replace("<", "_")
                .Replace(">", "_");

            IEnumerable<string> publicMembers = typeSymbol
                .GetMembers()
                .Where(x => x is (IFieldSymbol or IPropertySymbol)
                             and { IsStatic: false, DeclaredAccessibility: Accessibility.Public, IsImplicitlyDeclared: false, CanBeReferencedByName: true })
                .Select(x => $"{x.Name}:{{{x.Name}}}"); // MyProperty:{MyProperty}

            string toString = string.Join(", ", publicMembers);

            // multiline string interpolation
            string code = $$"""
// ========================== auto-generated
#pragma warning disable CS8600
#pragma warning disable CS8601
#pragma warning disable CS8602
#pragma warning disable CS8603
#pragma warning disable CS8604

namespace {{ns}}
{
    partial class {{typeSymbol.Name}}
    {
        public override string ToString()
        {
            return $"{{toString}}";
        }
    }
}
#pragma warning restore CS8604
#pragma warning restore CS8603
#pragma warning restore CS8602
#pragma warning restore CS8601
#pragma warning restore CS8600
""";

            context.AddSource($"{fullType}.SampleGenerator.g.cs", code);
        }
    }


    public static class DiagnosticDescriptors
    {
        private const string CATEGORY = "GeneratedNamespace";

        public static readonly DiagnosticDescriptor ExistsOverrideToString = new DiagnosticDescriptor(
            id: "SAMPLE001",
            title: "ToString override",
            messageFormat: "The GenerateToString class '{0}' has ToString override but it is not allowed",
            category: CATEGORY,
            defaultSeverity: DiagnosticSeverity.Error,
            isEnabledByDefault: true
        );
    }
}
```

### SourceGeneratorSrc

``` xml
<!-- SourceGeneratorSrc/SourceGeneratorSrc.csproj -->
<!-- TODO: https://stevetalkscode.co.uk/debug-source-generators-with-vs2019-1610 -->
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netstandard2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <ProjectReference Include="..\SourceGeneratorGen\SourceGeneratorGen.csproj" OutputItemType="Analyzer" ReferenceOutputAssembly="false" />
  </ItemGroup>
</Project>
```

``` csharp
// SourceGeneratorSrc/Program.cs

using GeneratedNamespace;
using System;

namespace SourceGeneratorSrc
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            MyClass mc = new MyClass() { Hoge = 10, Bar = "tako" };
            Console.WriteLine(mc);
        }
    }

    [GenerateToString]
    public partial class MyClass
    {
        public int Hoge { get; set; }
        public string Bar { get; set; }
    }
}
```

## Etc

``` csharp
// IDE 경고내기

private const string CATEGORY = "GeneratedNamespace";
public static readonly DiagnosticDescriptor ExistsOverrideToString = new DiagnosticDescriptor(
    id:            "SAMPLE001",
    title:         "ToString override",
    messageFormat: "The GenerateToString class '{0}' has ToString override but it is not allowed",
    category: CATEGORY,
    defaultSeverity: DiagnosticSeverity.Error,
    isEnabledByDefault: true
);

SourceProductionContext context;
context.ReportDiagnostic(Diagnostic.Create(DiagnosticDescriptors.ExistsOverrideToString, typeNode.Identifier.GetLocation(), typeSymbol.Name));
```

| ReportDiagnostic Enum |     |                                                                         |
| --------------------- | --- | ----------------------------------------------------------------------- |
| Default               | 0   | Report a diagnostic by default.                                         |
| Error                 | 1   | Report a diagnostic as an error.                                        |
| Warn                  | 2   | Report a diagnostic as a warning even though /warnaserror is specified. |
| Info                  | 3   | Report a diagnostic as an info.                                         |
| Hidden                | 4   | Report a diagnostic as hidden.                                          |
| Suppress              | 5   | Suppress a diagnostic.                                                  |


## 로슬린

컴파일

## Ref

- <https://docs.unity3d.com/Manual/roslyn-analyzers.html>
  - [Microsoft.CodeAnalysis 3.8](https://www.nuget.org/packages/Microsoft.CodeAnalysis.CSharp/3.8.0)
  - 문서상 3.8로 되어있지만 3.9로 해야함.
- [ISourceGenerator Interface](https://learn.microsoft.com/en-us/dotnet/api/microsoft.codeanalysis.isourcegenerator?view=roslyn-dotnet-4.7.0)
- [Source Generators](https://learn.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/source-generators-overview)
  - deprecated - ISourceGenerator
    - [Source Generators Cookbook](https://github.com/dotnet/roslyn/blob/main/docs/features/source-generators.cookbook.md)
  -IIncrementalGenerator
    - [Incremental Generators](https://github.com/dotnet/roslyn/blob/main/docs/features/incremental-generators.md)
- 정성태
  - [C# -Version 1 Source Generator 실습](https://www.sysnet.pe.kr/2/0/12985)
- [C# Source Generators - Write Code that Writes Code](https://www.youtube.com/watch?v=3YwwdoRg2F4)
- neue.cc
  - [2022年のC# (Incremental) Source Generator開発手法](https://neue.cc/2022/12/16_IncrementalSourceGenerator.html)
- Roslyn package version & Minimum supported Visual Studio version
  - <https://learn.microsoft.com/en-us/visualstudio/extensibility/roslyn-version-support?view=vs-2022>
- <https://andrewlock.net/exploring-dotnet-6-part-9-source-generator-updates-incremental-generators/>


- Set as Startup Project 간단히 : project 이름 > right click > `a`
- [Static anonymous functions](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-9.0/static-anonymous-functions)
  - static lamda면 외부 context를 캡쳐하지 않는다.

## Source Generator ex

- [Series: Creating a source generator](https://andrewlock.net/series/creating-a-source-generator/)
  - [Part 1 - Creating an incremental generator](https://andrewlock.net/creating-a-source-generator-part-1-creating-an-incremental-source-generator/)
  - [Part 2 - Testing an incremental generator with snapshot testing](https://andrewlock.net/creating-a-source-generator-part-2-testing-an-incremental-generator-with-snapshot-testing/)
  - [Part 3 - Integration testing and NuGet packaging](https://andrewlock.net/creating-a-source-generator-part-3-integration-testing-and-packaging/)
  - [Part 4 - Customising generated code with marker attributes](https://andrewlock.net/creating-a-source-generator-part-4-customising-generated-code-with-marker-attributes/)
  - [Part 5 - Finding a type declaration's namespace and type hierarchy](https://andrewlock.net/creating-a-source-generator-part-5-finding-a-type-declarations-namespace-and-type-hierarchy/)
  - [Part 6 - Saving source generator output in source control](https://andrewlock.net/creating-a-source-generator-part-6-saving-source-generator-output-in-source-control/)
  - [Part 7 - Solving the source generator 'marker attribute' problem - Part 1](https://andrewlock.net/creating-a-source-generator-part-7-solving-the-source-generator-marker-attribute-problem-part1/)
  - [Part 8 - Solving the source generator 'marker attribute' problem - Part 2](https://andrewlock.net/creating-a-source-generator-part-8-solving-the-source-generator-marker-attribute-problem-part2/)

## 