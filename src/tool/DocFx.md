
- https://github.com/dotnet/docfx
  - https://dotnet.github.io/docfx/index.html
  - https://dotnet.github.io/docfx/docs/markdown.html

- docfx
  - https://github.com/Cysharp/UniTask/blob/master/docs/docfx.json


docfx
https://github.com/ScottPlot/ScottPlot/tree/main/dev/docfx


``` sh
dotnet tool install --global docfx

docfx init --yes --output docs

# 빌드
docfx docs/docfx.json

# 서비스
docfx docs/docfx.json --serve

# 와치
# 아직까진 watch 옵션이 없다.
```


``` json
// docfx.json

"metadata": [
    {
      "src": [
        {
          "src": "../Hello",
          "files": [
            "**/*.csproj"
          ]
        }
      ],
      "dest": "api"
    }
  ],
```

docs/.gitignore
    .cache
    /**/_site/


- com.unity.package-manager-doctools
  - https://docs.unity3d.com/Packages/com.unity.package-manager-doctools@3.3/manual/index.html
  - PMDT - Package Manager Documentation Tools
  - dotnet tool install docfx --version <preferred version> --tool-path <user profile directory>/.pmdt
  - dotnet tool update  docfx --version <preferred version> --tool-path <user profile directory>/.pmdt
