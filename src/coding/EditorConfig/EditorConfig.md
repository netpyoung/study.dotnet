# EditorConfig

## Analyzers

- IDE
  - https://learn.microsoft.com/ko-kr/dotnet/fundamentals/code-analysis/style-rules/language-rules
- CA2007
  - Code analysis 
  - https://learn.microsoft.com/ko-kr/dotnet/fundamentals/code-analysis/quality-rules/design-warnings
- JSON
  - JSON001		Invalid JSON pattern	
  - JSON002		Probable JSON string detected
- RE
  - RE0001	Invalid regex pattern
- RCS
  - Roslynator.Analyzers
  - https://github.com/dotnet/roslynator
  - http://pihrt.net/Roslynator/Analyzers
  - https://josefpihrt.github.io/docs/roslynator/analyzers
- VSTHRD
  - Microsoft.VisualStudio.Threading.Analyzers
  - https://github.com/microsoft/vs-threading/blob/main/doc/analyzers/index.md
- SA
  - StyleCopAnalyzers
  - https://github.com/DotNetAnalyzers/StyleCopAnalyzers/tree/master/documentation

## Ref

- <https://docs.unity3d.com/Packages/com.unity.coding@0.1/manual/index.html>
  - 별로일듯 구성하는게 더빠름
- <https://github.com/CortiWins/UnityEditorConfigPostprocessor>



# editorconfig

- Visual Studio 설정
  - `Tools > Options > Text Editor > General > Follow project coding conventions`
  - [editorconfig-settings](https://docs.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options?view=vs-2019#troubleshoot-editorconfig-settings)

| 규칙           | 설명                                |
| -------------- | ----------------------------------- |
| 언어 규칙      | 한정자 및 괄호 구문 (.NET / C#)     |
| 서식 지정 규칙 | 들여쓰기, 공백, newline (.NET / C#) |
| 이름 지정 규칙 | naming                              |

``` txt
# EditorConfig is awesome: https://EditorConfig.org
root = true

[*.cs]
indent_style             = space
indent_size              = 4
end_of_line              = crlf           // lf / cr 
charset                  = utf-8          // utf-8-bom
trim_trailing_whitespace = true
insert_final_newline     = false
max_line_length          = 120

## :silent, :suggestion
## ===========================================================================
## ## 언어 규칙 - .NET 스타일 규칙
## ref: https://docs.microsoft.com/ko-kr/dotnet/fundamentals/code-analysis/style-rules/language-rules#net-style-rules
## ===========================================================================

## =============== `this.` 및 `Me.` 한정자
dotnet_style_qualification_for_field = true                                # <<<<<<<<<<<<<<<<<<<
dotnet_style_qualification_for_property = false
dotnet_style_qualification_for_method = false
dotnet_style_qualification_for_event = false

## =============== 형식 참조를 위한 프레임워크 형식 이름 대신 언어 키워드
dotnet_style_predefined_type_for_locals_parameters_members = true
dotnet_style_predefined_type_for_member_access = true

## =============== 한정자 기본 설정
dotnet_style_require_accessibility_modifiers = for_non_interface_members
csharp_preferred_modifier_order = public,private,protected,internal,static,extern,new,virtual,abstract,sealed,override,readonly,unsafe,volatile,async
dotnet_style_readonly_field = true

## =============== 괄호 기본 설정
dotnet_style_parentheses_in_arithmetic_binary_operators = always_for_clarity
dotnet_style_parentheses_in_relational_binary_operators = always_for_clarity
dotnet_style_parentheses_in_other_binary_operators = always_for_clarity
dotnet_style_parentheses_in_other_operators = never_if_unnecessary

## =============== 식 수준 기본 설정
dotnet_style_object_initializer = true
dotnet_style_collection_initializer = true
dotnet_style_explicit_tuple_names = true
dotnet_style_prefer_inferred_tuple_names = true
dotnet_style_prefer_inferred_anonymous_type_member_names = true
dotnet_style_prefer_auto_properties = true
dotnet_style_prefer_conditional_expression_over_assignment = false       # <<<<<<<<<<<<<<<<<<<
dotnet_style_prefer_conditional_expression_over_return = false           # <<<<<<<<<<<<<<<<<<<
dotnet_style_prefer_compound_assignment = true
dotnet_style_prefer_simplified_interpolation = true
dotnet_style_prefer_simplified_boolean_expressions = true

## =============== Null 검사 기본 설정
dotnet_style_coalesce_expression = false                                 # <<<<<<<<<<<<<<<<<<<
dotnet_style_null_propagation = false                                    # <<<<<<<<<<<<<<<<<<<
dotnet_style_prefer_is_null_check_over_reference_equality_method = true

## =============== 파일 헤더 기본 설정
file_header_template = unset


## ===========================================================================
## ## 언어 규칙 - C# 스타일 규칙
## ref: https://docs.microsoft.com/ko-kr/dotnet/fundamentals/code-analysis/style-rules/language-rules#c-style-rules
## ===========================================================================

## =============== `var` 기본 설정
csharp_style_var_for_built_in_types = false                              # <<<<<<<<<<<<<<<<<<<
csharp_style_var_when_type_is_apparent = false                           # <<<<<<<<<<<<<<<<<<<
csharp_style_var_elsewhere = false                                       # <<<<<<<<<<<<<<<<<<<

## =============== 식 본문 멤버
csharp_style_expression_bodied_methods = when_on_single_line             # <<<<<<<<<<<<<<<<<<<
csharp_style_expression_bodied_constructors = when_on_single_line        # <<<<<<<<<<<<<<<<<<<
csharp_style_expression_bodied_operators = when_on_single_line           # <<<<<<<<<<<<<<<<<<<
csharp_style_expression_bodied_properties = when_on_single_line          # <<<<<<<<<<<<<<<<<<<
csharp_style_expression_bodied_indexers = when_on_single_line            # <<<<<<<<<<<<<<<<<<<
csharp_style_expression_bodied_accessors = when_on_single_line           # <<<<<<<<<<<<<<<<<<<
csharp_style_expression_bodied_lambdas = when_on_single_line             # <<<<<<<<<<<<<<<<<<<
csharp_style_expression_bodied_local_functions = false

## =============== 패턴 일치 기본 설정
csharp_style_pattern_matching_over_is_with_cast_check = true
csharp_style_pattern_matching_over_as_with_null_check = true
csharp_style_prefer_switch_expression = false                            # <<<<<<<<<<<<<<<<<<<
csharp_style_prefer_pattern_matching = false                             # <<<<<<<<<<<<<<<<<<<
csharp_style_prefer_not_pattern = false                                  # <<<<<<<<<<<<<<<<<<<

## =============== 식 수준 기본 설정
csharp_style_inlined_variable_declaration = true
csharp_prefer_simple_default_expression = false                          # <<<<<<<<<<<<<<<<<<<
csharp_style_pattern_local_over_anonymous_function = true
csharp_style_deconstructed_variable_declaration = false                  # <<<<<<<<<<<<<<<<<<<
csharp_style_prefer_index_operator = false                               # <<<<<<<<<<<<<<<<<<<
csharp_style_prefer_range_operator = false                               # <<<<<<<<<<<<<<<<<<<
csharp_style_implicit_object_creation_when_type_is_apparent = false      # <<<<<<<<<<<<<<<<<<<

## =============== "Null" 검사 기본 설정
csharp_style_throw_expression = false                                    # <<<<<<<<<<<<<<<<<<<
csharp_style_conditional_delegate_call = false                           # <<<<<<<<<<<<<<<<<<<

## =============== 코드 블록 기본 설정
csharp_prefer_braces = true
csharp_prefer_simple_using_statement = false                             # <<<<<<<<<<<<<<<<<<<

## =============== `using` 지시문 기본 설정
csharp_using_directive_placement = outside_namespace

## =============== 한정자 기본 설정
csharp_prefer_static_local_function = true:suggestion


## ===========================================================================
## ## 서식 지정 규칙 - .NET 서식 지정 규칙
## ref: https://docs.microsoft.com/ko-kr/dotnet/fundamentals/code-analysis/style-rules/formatting-rules#net-formatting-rules
## ===========================================================================

## =============== using 지시문 구성
dotnet_sort_system_directives_first = true
dotnet_separate_import_directive_groups = true


## ===========================================================================
## ## 서식 지정 규칙 - C# 서식 지정 규칙
## ref: https://docs.microsoft.com/ko-kr/dotnet/fundamentals/code-analysis/style-rules/formatting-rules#c-formatting-rules
## ===========================================================================

## =============== 줄 바꿈 옵션
csharp_new_line_before_open_brace = all                                 # // none
csharp_new_line_before_else = true
csharp_new_line_before_catch = true
csharp_new_line_before_finally = true
csharp_new_line_before_members_in_object_initializers = true
csharp_new_line_before_members_in_anonymous_types = true
csharp_new_line_between_query_expression_clauses = true

## =============== 들여쓰기 옵션
csharp_indent_case_contents = true
csharp_indent_switch_labels = true
csharp_indent_labels= no_change
csharp_indent_block_contents = true
csharp_indent_braces = false
csharp_indent_case_contents_when_block = false

## =============== 간격 옵션
csharp_space_after_cast = false
csharp_space_after_keywords_in_control_flow_statements = true
csharp_space_between_parentheses = none                                 # // control_flow_statements, expressions 또는 type_casts 이외의 값을 사용하면 설정이 적용되지 않습니다
csharp_space_before_colon_in_inheritance_clause = true
csharp_space_after_colon_in_inheritance_clause = true
csharp_space_around_binary_operators = before_and_after
csharp_space_between_method_declaration_parameter_list_parentheses = false
csharp_space_between_method_declaration_empty_parameter_list_parentheses = false
csharp_space_between_method_declaration_name_and_open_parenthesis = false
csharp_space_between_method_call_parameter_list_parentheses = false
csharp_space_between_method_call_empty_parameter_list_parentheses = false
csharp_space_between_method_call_name_and_opening_parenthesis = false

csharp_space_after_comma = true
csharp_space_before_comma = false
csharp_space_after_dot = false
csharp_space_before_dot = false

csharp_space_after_semicolon_in_for_statement = true
csharp_space_before_semicolon_in_for_statement = false
csharp_space_around_declaration_statements = ignore                      # // ignore | false  (선언문에서 추가 공백 문자를 제거 여부. false로 하기에는 포맷 맞추는 일이 있을지도)
csharp_space_before_open_square_brackets = false
csharp_space_between_empty_square_brackets = false
csharp_space_between_square_brackets = false

## =============== 줄 바꿈 옵션
csharp_preserve_single_line_statements = false
csharp_preserve_single_line_blocks = true

## =============== 지시문 옵션 사용
csharp_using_directive_placement = outside_namespace


## ===========================================================================
## ## 이름 지정 규칙
## ref: https://docs.microsoft.com/ko-kr/dotnet/fundamentals/code-analysis/style-rules/naming-rules
## ===========================================================================

```



## Etc

http://trapezoid.hatenablog.com/entry/2015/12/08/231845

Code Formatter
https://github.com/icsharpcode/NRefactory/tree/master/ICSharpCode.NRefactory.CSharp/Formatter
