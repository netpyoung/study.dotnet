## TODO Publish

https://learn.microsoft.com/en-us/dotnet/core/deploying/?pivots=visualstudio

InvariantGlobalization - true - 고정 모드에서 실행 📉
https://learn.microsoft.com/ko-kr/dotnet/core/runtime-config/globalization


SelfContained - 런타임 포함 - 파일사이즈 커짐📈
EnableCompressionInSingleFile - managed 코드 압축 📉 - SelfContained 일 때만사용가능 - Compression in a single file bundle is only supported when publishing a self-contained application.
PublishTrimmed - DLL속 프로그램에서 사용 중인 것만 포함 📉
DebugType - embedded -  pdb 실행파일에 포함 📈


PublishSingleFile - PublishAot랑 같이 못씀
PublishAot - 런타임 내장 (IL을 완전히 네이티브 코드로 바꿈) 📈
PublishReadyToRun - 런타임 필요 (IL을 일부만 네이티브 코드로 바꿈) - 파일사이즈 커짐📈


OptimizationPreference - Size - AOT전용 📉 - https://learn.microsoft.com/en-us/dotnet/core/deploying/native-aot/optimizing


https://learn.microsoft.com/en-us/dotnet/core/deploying/trimming/trimming-options
PublishTrimmed - true - 📉 Windows Forms is not supported or recommended with trimming enabled. Please go to https://aka.ms/dotnet-illink/windows-forms for more details.
_SuppressWinFormsTrimError - true - 위 경고 우회용


