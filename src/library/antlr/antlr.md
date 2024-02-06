ANTLR (ANother Tool for Language Recognition

parser geenrator

렉서(토큰화) 및 파서(구문 분석) 단계를 모두 지원


AST탐색 방식
- visitor
  - 각 노드가 하위 노드를 방문하는 함수를 직접 호출
- listener
  - 각 노드를 방문/탈출시 그에 해당하는 함수가 호출됨


https://putridparrot.com/blog/antlr-in-c/


~~~
java org.antlr.v4.Tool
dotnet new console
dotnet new classlib
dotnet new sln
dotnet sln add
Install-Package Antlr4.Runtime.Standard
~~~

# Ref:

## Ref

- https://www.slideshare.net/Chun92/abstract-syntax-semantic-analyze
- <https://fullboarllc.com/antlr4-dotnet-core-visitor/>
- <https://github.com/antlr/antlr4/tree/master/runtime/CSharp>
- <https://www.antlr.org/download.html>
  - <https://github.com/antlr/antlr4/blob/master/doc/csharp-target.md>
- https://fullboarllc.com/using-antlr-4-with-net-core-2-1-and-c-getting-started/

- https://medium.com/@isetitra/step-by-step-creation-of-a-simple-compiler-using-antlr4-9285755cf943

https://github.com/netpyoung/study.antlr4/