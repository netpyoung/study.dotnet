# InnoSetup

https://jrsoftware.org/isinfo.php

- 다운로드: https://jrsoftware.org/isdl.php
- help: https://jrsoftware.org/ishelp/
- source: https://github.com/jrsoftware/issrc
  - Compil32.exe GUI front-end.
  - ISCC.exe - command-line front-end
- github action: [Minionguyjpro/Inno-Setup-Action](https://github.com/Minionguyjpro/Inno-Setup-Action)
- 언팩: https://github.com/dscharrer/innoextract

## 라이센스

- https://jrsoftware.org/isorder.php#commercialuser

연간 수익이 5,000달러를 초과하거나 다른 통화로 이에 상응하는 금액을 초과하는 영리 조직입니다.
연간 수익이 5,000달러를 초과하거나 다른 통화로 환산된 금액에 달하는 수익 목적의 업무의 일환으로 Inno Setup을 사용하는 개인/프리랜서
오픈 소스/프리웨어 소프트웨어에 대한 개인/조직의 금전적 기부는 수익 한도에 포함됩니다.

- https://jrsoftware.org/isorder.php#required

엄격하게 요구되는 사항은 아닙니다

## GUID

Compil32에서 Tools > Generate GUID


.isl - Inno Setup Language

Inno Setup의 다국어 지원용 메시지/문자열 파일이라고

## .iss - Inno Setup Script

- https://jrsoftware.org/ishelp/topic_consts.htm

{app}
The application directory, which the user selects on the Select Destination Location page of the wizard.
{tmp}
Temporary directory used by Setup or Uninstall. This is not the value of the user's TEMP environment variable. It is a subdirectory of the user's temporary directory which is created by Setup or Uninstall at startup (with a name like "C:\WINDOWS\TEMP\IS-xxxxx.tmp"). All files and subdirectories in this directory are deleted when Setup or Uninstall exits. During Setup, this is primarily useful for extracting files that are to be executed in the [Run] section but aren't needed after the installation.
{group}
{autodesktop}


https://jrsoftware.org/ishelp/topic_custommessagessection.htm
{cm:...}

Inno Setup의 **스크립트 파서(parser)**는 등호(=) 주변의 공백을 무시하도록 설계되어 있습니다.

[Setup]
AppId
The length of AppId with all constants evaluated should never exceed 127 characters.


ISPP Inno Setup PreProcessor
https://jrsoftware.org/ishelp/topic_directives.htm

AppID쪽 guid 괄호 처리가 이상하다
https://stackoverflow.com/questions/10303551/extra-character-added-using-emit-setupsettingappid-in-inno-setup

문자열 리터럴 `{{`, `}}`
https://jrsoftware.org/ishelp/index.php?topic=consts
A "{" character is treated as the start of the constant. If you want to use that actual character in a place where constants are supported, you must use two consecutive "{" characters. (You do not need to double "}" characters.)




ISCC.exe main.iss


app-icon.ico - 16x16, 32x32, 48x48, 64x64, and 256x256. https://jrsoftware.org/ishelp/topic_setup_setupiconfile.htm
main.iss
define.iss


define.iss
APP_ID

main.iss
#include "define.iss"

[Setup]
[Languages]
[Tasks]


[Icons]
[Registry]


파일 설치

[Files]
Source: "..\buildmachine\a_dir\*";           DestDir: "{app}\a_dir";   Flags: ignoreversion recursesubdirs
Source: "..\buildmachine\b_dir\b.txt";       DestDir: "{app}\b_dir";   Flags: ignoreversion

파일 설치 및 실행
[Files]
Source: "..\buildmachine\to_temp\temp-hello.exe";    DestDir: {tmp};

[Run]
Filename: "{tmp}\temp-hello.exe";                    Description: test hello;   Flags: postinstall
