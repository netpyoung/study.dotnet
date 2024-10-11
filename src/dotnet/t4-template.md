
.tt


- TextTransform .exe
  - https://learn.microsoft.com/ko-kr/visualstudio/modeling/generating-files-with-the-texttransform-utility?view=vs-2022
  - \Program Files\Microsoft Visual Studio\2022\Community\Common7\IDE
  - \Program Files\Microsoft Visual Studio\2022\Professional\Common7\IDE
  - \Program Files\Microsoft Visual Studio\2022\Enterprise\Common7\IDE






- T4 `T`ext `T`emplate `T`ransformation `T`oolkit

https://learn.microsoft.com/ko-kr/visualstudio/modeling/code-generation-and-t4-text-templates?view=vs-2022


package System.CodeDom

|                       | Custom Tool                    |                                               |
| --------------------- | ------------------------------ | --------------------------------------------- |
| Runtime Text Tempalte | TextTemplatingFilePreprocessor | C#에서 템플릿 파일명을 클래스명으로 접근 가능 |
| Text Template         | TextTemplatingFileGenerator    |                                               |


[T4를 이용한 INotifyPropertyChanged 코드 자동 생성](https://www.sysnet.pe.kr/2/0/1619)

Liquid
https://shopify.github.io/liquid/
https://github.com/lekman/AzureLiquid

``` xml
<!-- 
Ref: https://learn.microsoft.com/en-us/visualstudio/msbuild/item-element-msbuild?view=vs-2022#attributes

-->

  <ItemGroup>
  
    <None Update="TextTemplate1.tt">
      <Generator>TextTemplatingFileGenerator</Generator>
      <LastGenOutput>TextTemplate1.txt</LastGenOutput>
    </None>
    <None Update="TextTemplate1.txt">
      <DesignTime>True</DesignTime>
      <AutoGen>True</AutoGen>
      <DependentUpon>TextTemplate1.tt</DependentUpon>
    </None>
  </ItemGroup>

  <ItemGroup>
    <Service Include="{508349b6-6b84-4df5-91f0-309beebad82d}" />
  </ItemGroup>
```
  <ItemGroup>
    <None Update="RuntimeTextTemplate1.tt">
      <Generator>TextTemplatingFilePreprocessor</Generator>
      <LastGenOutput>RuntimeTextTemplate1.cs</LastGenOutput>
    </None>
  </ItemGroup>

  <ItemGroup>

<!--
Ref:
https://learn.microsoft.com/en-us/previous-versions/visualstudio/visual-studio-2010/bb932394(v=vs.100)

[GuidAttribute("508349B6-6B84-4df5-91F0-309BEEBAD82D")]
[ComVisibleAttribute(true)]
[CLSCompliantAttribute(true)]
public interface STextTemplating
-->
  >
    <Service Include="{508349b6-6b84-4df5-91f0-309beebad82d}" />
  </ItemGroup>

  <ItemGroup>
    <Compile Update="RuntimeTextTemplate1.cs">
      <DesignTime>True</DesignTime>
      <AutoGen>True</AutoGen>
      <DependentUpon>RuntimeTextTemplate1.tt</DependentUpon>
    </Compile>
  </ItemGroup>
## 디버깅

- Solution Explorer > .tt > 우클릭 > Debug T4 Template
- 혹은

``` xml 
<#@ template debug="true" hostSpecific="true" #>
<#@ import namespace="System.Diagnostics" #>
<# Debugger.Launch(); #>
```

DBCSCodePageEncoding은 Double Byte Character Set (DBCS) 코드 페이지 인코딩을

<#@ DirectiveName [AttributeName = "AttributeValue"] ... #>
<# Standard control blocks #> 문장을 포함할 수 있습니다.
<#= Expression control blocks # >표현식을 포함할 수 있습니다.
<#+ Class feature control blocks #>메서드, 필드, 속성을 포함할 수 있습니다.


https://marketplace.visualstudio.com/items?itemName=bricelam.T4Language
https://github.com/bricelam/T4Language


https://marketplace.visualstudio.com/items?itemName=bricelam.VSDataSqlite
https://github.com/bricelam/VS.Data.Sqlite

<#@ template debug="false" hostspecific="false" language="C#" #>
<#@ output extension=".cs" #>


hostspecific="true": 템플릿에서 호스트 애플리케이션의 환경 정보, 파일 경로 등 호스트와 관련된 정보를 사용할 수 있습니다.
    // ex) 템플릿이 실행되는 디렉터리 경로를 가져옴
