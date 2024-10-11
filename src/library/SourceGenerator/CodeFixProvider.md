
``` cs
[ExportCodeFixProvider(LanguageNames.CSharp, Name = nameof(MyAnalyzerCodeFixProvider)), Shared]
public class MyAnalyzerCodeFixProvider : CodeFixProvider
```

 View -> Other Windows -> Syntax Visualizer.


https://learn.microsoft.com/en-us/dotnet/api/microsoft.codeanalysis.diagnostics.generatedcodeanalysisflags?view=roslyn-dotnet-4.9.0
[System.Flags]
public enum GeneratedCodeAnalysisFlags


https://github.com/microsoft/Microsoft.Unity.Analyzers


csc.rsp file (Assets/csc.rsp):
-ruleset:Assets/Default.ruleset

[PredefinedAssemblyName].ruleset
https://docs.unity3d.com/kr/2021.3/Manual/roslyn-analyzers.html



using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp.Syntax;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.Diagnostics;
using Microsoft.CodeAnalysis.Text;

https://github.com/dotnet/roslyn/blob/main/docs/wiki/How-To-Write-a-C%23-Analyzer-and-Code-Fix.md
https://www.meziantou.net/writing-a-roslyn-analyzer.htm
https://learn.microsoft.com/en-us/visualstudio/code-quality/roslyn-analyzers-overview?view=vs-2022



DiagnosticDescriptor



// dotnet/runtime generators.

// https://github.com/dotnet/runtime/blob/main/src/libraries/System.Text.RegularExpressions/gen/
// https://github.com/dotnet/runtime/tree/main/src/libraries/System.Text.Json/gen
// https://github.com/dotnet/runtime/tree/main/src/libraries/System.Private.CoreLib/gen
// https://github.com/dotnet/runtime/tree/main/src/libraries/Microsoft.Extensions.Logging.Abstractions/gen
// https://github.com/dotnet/runtime/tree/main/src/libraries/System.Runtime.InteropServices.JavaScript/gen/JSImportGenerator
// https://github.com/dotnet/runtime/tree/main/src/libraries/System.Runtime.InteropServices/gen/LibraryImportGenerator
// https://github.com/dotnet/runtime/tree/main/src/tests/Common/XUnitWrapperGenerator

// documents, blogs.

// https://github.com/dotnet/roslyn/blob/main/docs/features/incremental-generators.md
// https://andrewlock.net/creating-a-source-generator-part-1-creating-an-incremental-source-generator/
// https://qiita.com/WiZLite/items/48f37278cf13be899e40
// https://zenn.dev/pcysl5edgo/articles/6d9be0dd99c008
// https://neue.cc/2021/05/08_600.html
// https://www.thinktecture.com/en/net/roslyn-source-generators-introduction/

// for check generated file
// <EmitCompilerGeneratedFiles>true</EmitCompilerGeneratedFiles>
// <CompilerGeneratedFilesOutputPath>Generated</CompilerGeneratedFilesOutputPath>



Microsoft.Build.Utilities // ToolTask
https://github.com/dotnet/msbuild/blob/main/src/Utilities/ToolTask.cs
https://learn.microsoft.com/en-us/dotnet/api/microsoft.build.utilities.tooltask?view=msbuild-17-netcore
https://github.com/grpc/grpc/blob/master/src/csharp/Grpc.Tools/ProtoCompile.cs