| 속성 이름                    | 설명                                                          |
| ---------------------------- | ------------------------------------------------------------- |
| `$(ProjectDir)`              | 현재 프로젝트의 디렉토리 경로                                 |
| `$(SolutionDir)`             | 솔루션 파일이 위치한 디렉토리 경로                            |
| `$(OutputPath)`              | 빌드 결과물이 저장되는 경로 (기본값: `bin\$(Configuration)\`) |
| `$(TargetDir)`               | 빌드된 출력 파일이 위치한 폴더 (`OutputPath`와 동일)          |
| `$(TargetFramework)`         | 현재 타겟 프레임워크 (예: `net5.0`, `netcoreapp3.1` 등)       |
| `$(Configuration)`           | 현재 빌드 구성 (예: `Debug`, `Release`)                       |
| `$(ProjectName)`             | 현재 프로젝트의 이름                                          |
| `$(SolutionName)`            | 현재 솔루션의 이름                                            |
| `$(MSBuildProjectName)`      | 현재 MSBuild 프로젝트의 이름                                  |
| `$(MSBuildProjectDirectory)` | 현재 MSBuild 프로젝트의 디렉토리 경로                         |
| `$(NuGetPackageRoot)`        | NuGet 패키지가 저장되는 루트 경로                             |



https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props#runworkingdirectory


<PropertyGroup>
  <RunArguments>-mode dryrun</RunArguments>
  <RunWorkingDirectory>c:\temp</RunWorkingDirectory>
</PropertyGroup>