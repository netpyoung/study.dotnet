# Avalonia

https://github.com/robloo/PublicDocs/blob/master/XAMLFrameworkComparison.md

https://xaml.io/


https://avaloniaui.net/
https://avaloniaui.net/gettingstarted#installation

- https://avaloniaui.net/accelerate


https://github.com/AvaloniaUI/avalonia-dotnet-templates

``` cmd
dotnet new install Avalonia.Templates

dotnet new list | grep avalonia

dotnet new avalonia.app -o MyApp
```


``` xml
https://github.com/AvaloniaUI/Avalonia/wiki/Using-nightly-build-feed

	<PropertyGroup>
        <!-- 정식릴리즈 말고 다른 버전 테스트 -->
		<RestoreSources>
			https://nuget-feed-all.avaloniaui.net/v3/index.json;
			https://api.nuget.org/v3/index.json;
			$(RestoreSources)
		</RestoreSources>
		<AvaloniaVersion>12.0.999-cibuild0057950-alpha</AvaloniaVersion>
	</PropertyGroup>
```

---

xmlns - xml namespace
xmlns:x - 접두사 `x:` 선언 - https://learn.microsoft.com/en-us/dotnet/desktop/xaml-services/namespace-language-features
x:Class - https://learn.microsoft.com/en-us/dotnet/desktop/xaml-services/xclass-directive
  - 이 XAML이 정의하는 실제 C# 클래스
  - Text="{Binding Header, RelativeSource={RelativeSource AncestorType=local:GroupBox}}" 이런식으로 DataType없이 바인딩 테스트 가능
    - https://learn.microsoft.com/en-us/dotnet/desktop/wpf/advanced/relativesource-markupextension
    - https://docs.avaloniaui.net/docs/basics/data/data-binding/data-binding-syntax

x:DataType - https://learn.microsoft.com/en-us/dotnet/maui/fundamentals/data-binding/compiled-bindings
  - {Binding} / {x:Bind}랑 같이 쓰임
  - XAML 컴파일러와 디자인 타임 힌트용 (런타임에는 영향 없음)
  - Avalonia에서는 그냥 {Binding} 으로 퉁치는듯
    - https://docs.avaloniaui.net/docs/basics/data/data-binding/compiled-bindings
    - Avalonia는 “WPF를 그대로 가져오되, 최신 기능을 얹는다” 라는 전략을 택했습니다.
    - {x:Bind}는 UWP 전용 문법이므로, WPF와의 호환성을 깨지 않기 위해 도입하지 않았습니다.

xmlns:d - Visual Studio의 디자이너(Design view) 에서만 사용하는 속성들을 정의 - 즉, **런타임(실행 시)**에는 완전히 무시
d:DataContext - 디자인 타임 데이터 바인딩 설정

xmlns:mc - Markup Compatibility(마크업 호환성) 네임스페이스
mc:Ignorable="d"의 의미 - 이건 “d:로 시작하는 속성이나 요소는 실행 시 무시


xmlns:vm="using:MyApp" - namespace를 vm별칭으로 가져오겠다는말 - "vm"은 관례적으로 ViewModel
x:DataType="vm:GroupBox" - 컴파일 타임 바인딩(Compiled Bindings)


- How To Use Design-time Data
  - https://docs.avaloniaui.net/docs/guides/implementation-guides/how-to-use-design-time-data


Styles
TemplatedControl
UserControl


<Application RequestedThemeVariant="Default">

<Window Title="MyApp">

<UserControl>

---

## Snippet

### 콘솔 출력확인용 콘솔

``` xml
<OutputType>WinExe</OutputType> => <OutputType>Exe</OutputType>
```


### 파일 오픈

- https://docs.avaloniaui.net/docs/basics/user-interface/file-dialogs

### 드래그 드랍

axaml 쪽에 DragDrop.AllowDrop="True"

``` cs

InitializeComponent();
AddHandler(DragDrop.DropEvent, Drop);

    private void Drop(object? sender, DragEventArgs e)
    {
        // IEnumerable<IStorageItem>? filesOrNull = e.Data.GetFiles();
        IEnumerable<IStorageItem>? filesOrNull = e.DataTransfer.TryGetFiles();
        if (filesOrNull == null)
        {
            return;
        }

        foreach (IStorageItem file in filesOrNull)
        {
            string? pathOrNull = file.TryGetLocalPath();
            Console.WriteLine(pathOrNull);
        }
    }

```

### macos

- https://docs.avaloniaui.net/docs/deployment/macOS

``` xml
<RuntimeIdentifier>osx-x64</RuntimeIdentifier> <!-- 또는 osx-arm64 -->
```

### hotreload

- https://github.com/Kira-NT/HotAvalonia

``` xml
<ItemGroup>
    <PackageReference Include="Avalonia.Markup.Xaml.Loader" Version="$(AvaloniaVersion)" />
    <PackageReference Include="HotAvalonia" Version="3.0.0" PrivateAssets="All" Publish="True" />
</ItemGroup>
```

### Custom Control

- https://docs.avaloniaui.net/docs/guides/custom-controls/how-to-create-advanced-custom-controls
- style example: https://docs.avaloniaui.net/docs/tutorials/groupbox

### MVVM

- CommunityToolKit
- ReactiveUI

### 테마

``` xml
<Application>
	<Application.Styles>
		<FluentTheme />
		<!--<SimpleTheme />-->
	</Application.Styles>
</Application>
```

- https://github.com/BAndysc/Classic.Avalonia

### 스타일

``` xml
	<Window.Styles>
		<Style Selector="Button">
			<Setter Property="Height" Value="50"/>
			<Setter Property="FontSize" Value="30"/>
		</Style>
	</Window.Styles>
```

### ViewModel 클래스 생성없이 바인딩

``` xml
xmlns:vm="using:HelloNamespace"
x:DataType="vm:HelloClass"

<TextBox Grid.Row="0" Grid.Column="1" Margin="5" Text="{Binding Hello, Mode=TwoWay}"/>
```

``` cs
public static readonly StyledProperty<string> HelloProperty = AvaloniaProperty.Register<UserControl, string>(nameof(Hello));

public string Hello
{
    get => GetValue(HelloProperty);
    set => SetValue(HelloProperty, value);
}

InitializeComponent();
DataContext = this;
```


### Avalonia 프로젝트 퍼블리쉬시 포함될 파일들

|                      |                                           |
| -------------------- | ----------------------------------------- |
| libHarfBuzzSharp.dll | 폰트 셰이핑 - https://harfbuzz.github.io/ |
| av_libglesv2.dll     | OpenGL ES <=> Direct3D 변환               |
| libSkiaSharp.dll     | 렌더링 엔진 - https://skia.org/           |
