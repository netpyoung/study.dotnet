

[MSIX](https://learn.microsoft.com/en-us/windows/msix/overview)

[WiX Toolset](https://www.firegiant.com/wixtoolset/)

InstallShield : 상용

[Nsis](./Nsis.md)


데스탑 앱을 배포시 .NET Desktop Runtime 이 필요할것이다
https://dotnet.microsoft.com/en-us/download/dotnet


## MacOS

dmg 배포

-volname → 마운트 시 보이는 이름
-format UDZO → 압축 DMG
hdiutil create -volname "MyApp" -srcfolder MyApp-dmg -ov -format UDZO MyApp.dmg

https://github.com/create-dmg/create-dmg
