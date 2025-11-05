
# MAUI

- 아직 Linux지원이 원활하지 않음.
- 라이브 프리뷰가 있긴한데, 실행중에 바꾸는거임

https://stackoverflow.com/a/72636993

Visual Studio Installer > Modify > .NET Muti-platform App UI development

``` xml
<TargetFrameworks>net6.0-maccatalyst</TargetFrameworks>
<TargetFrameworks Condition="$([MSBuild]::IsOSPlatform('windows'))">$(TargetFrameworks);net6.0-windows10.0.19041.0</TargetFrameworks>


<SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'maccatalyst'">14.0</SupportedOSPlatformVersion>
<SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'windows'">10.0.17763.0</SupportedOSPlatformVersion>
<TargetPlatformMinVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'windows'">10.0.17763.0</TargetPlatformMinVersion>\
```


Debug > Windows > XAML Live Preview.


https://xamgirl.com/exploring-drag-and-drop-in-xamarin-forms/

https://github.com/dotnet/maui/issues/6080


## Alternative

- Avalonia UI - skia
- UNO




<SatelliteResourceLanguages>none</SatelliteResourceLanguages> 로케일 폴더)를 완전히 제외
<SatelliteResourceLanguages>en-US</SatelliteResourceLanguages>  <!-- 영어만 지원. 필요 시 'ko;en-US'처럼 추가 -->

https://devblogs.microsoft.com/visualstudio/enhancements-to-xaml-live-preview-in-visual-studio-for-net-maui/?hide_banner=true