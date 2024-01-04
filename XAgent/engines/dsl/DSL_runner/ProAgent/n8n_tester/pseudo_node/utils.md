# FunctionDef format_expression
**format_expression函数**：此函数的功能是替换给定表达式中特定模式的出现。

`format_expression`函数接收一个字符串参数`expression`，该参数是需要被格式化的表达式。函数的主要作用是查找表达式中所有符合特定模式的部分，并将其替换为新的格式。这个特定模式是通过正则表达式定义的，用于匹配形如`{{ $... }}`的结构。

在这个函数中，定义了一个正则表达式模式`pattern`，它用于识别形如`{{ $变量名 }}`的表达式。每当找到一个匹配项时，`replace_match`函数会被调用，它将匹配项中的变量名提取出来，并重新构造成`$input.item.变量名`的形式，这种形式适用于n8n工作流中的变量引用。

接着，使用`re.sub`函数将所有匹配的模式替换为`replace_match`函数返回的新格式。最终，函数返回一个新的字符串，其中所有的`{{ $... }}`结构都被替换为了`$input.item...`形式。

在项目中，`format_expression`函数被调用于`XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/pseudo_node/utils.py`文件中的`replace_single_exp`函数里。在这个上下文中，`format_expression`用于格式化以等号`=`开头的表达式。这是因为在n8n工作流中，等号开头的表达式指示了一个需要被计算的表达式。`format_expression`函数帮助将这些表达式转换成n8n可以理解的格式，以便在工作流中执行。

**注意**：
- 使用此函数时，确保传入的`expression`字符串格式正确，以便正则表达式能够正确匹配和替换。
- 此函数依赖于Python的`re`模块进行正则表达式操作，因此在使用前需要确保已经导入了`re`模块。

**输出示例**：
如果传入的`expression`为`"Hello {{ $name }}, you have {{ $unread_messages }} unread messages."`，则`format_expression`函数的返回值可能为`"Hello $input.item.name, you have $input.item.unread_messages unread messages."`。
## FunctionDef replace_match
**replace_match 函数**: 此函数的功能是将正则表达式匹配的内容转换为特定的格式字符串。

replace_match 函数是一个辅助函数，用于在字符串替换操作中，将正则表达式匹配到的内容进行格式化。它被定义在 XAgent 项目的 DSL_runner 模块中的 ProAgent 的 n8n_tester 的 pseudo_node 的 utils.py 文件中，并且在同一文件的 format_expression 函数中被调用。

具体来说，replace_match 函数接收一个正则表达式匹配对象 match 作为参数。它从这个匹配对象中提取出第一个捕获组（即正则表达式中括号内匹配到的内容），并将其保存在变量 match_content 中。然后，函数返回一个格式化的字符串，该字符串将 match_content 嵌入到一个特定的模板中：`'${{$input.item.{match_content}}}'`。这个模板字符串是为了适配 n8n 工作流自动化平台中的表达式格式。

在 format_expression 函数中，replace_match 函数被用作 re.sub 方法的替换函数。re.sub 方法用于在给定的表达式字符串中查找所有符合特定正则表达式模式的部分，并对每个匹配使用 replace_match 函数进行替换。这样，原始表达式中的 `{{ $变量名 }}` 形式的占位符就会被转换为 n8n 所需的格式 `$input.item.变量名`。

**注意**:
- replace_match 函数设计为一个闭包函数，仅在 format_expression 函数内部使用，不应在其他地方直接调用。
- 正则表达式匹配对象 match 必须至少包含一个捕获组，否则 match.group(1) 将无法正确工作。
- 函数返回的字符串格式是为了适配 n8n 平台的特定语法，如果在其他上下文中使用，可能需要修改字符串模板以适应不同的需求。

**输出示例**:
假设有一个匹配对象 match，其 group(1) 返回 "变量名"，那么 replace_match(match) 的返回值将会是：
```
${{$input.item.变量名}}
```
这个输出示例展示了函数如何将捕获组中的内容替换到特定的字符串模板中。
***
# FunctionDef replace_single_exp
**replace_single_exp 函数**: 此函数的功能是替换给定数据项中的单个表达式。

此函数接收一个字典类型的`data_item`和一个字符串类型的`expression`作为参数。它的主要作用是在`data_item`中替换掉指定的`expression`表达式，并返回修改后的表达式。

详细分析如下：
- 如果`expression`以等号'='开头，表示这是一个需要执行替换的表达式。
- 首先，将等号后面的表达式内容进行格式化处理。
- 然后，使用格式化后的表达式和`data_item`生成一个替换表达式的工作流。
- 工作流被写入一个临时的JSON文件中，并使用`n8n`命令行工具执行。
- 执行的输出结果会被解析，以提取修改后的表达式。
- 如果执行成功且修改后的表达式非空，则返回该表达式。
- 如果执行失败或者修改后的表达式为空，则抛出`BaseException`异常。
- 如果`expression`不是以等号'='开头，那么它将被直接返回。

在项目中的调用情况：
此函数在`XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/pseudo_node/utils.py`文件中被`replace_exp_recursive`函数调用。在`replace_exp_recursive`函数中，它用于递归地替换`data_item`字典中与`expression_dict`字典相对应的表达式值。

**注意**：
- 使用此函数时，需要确保`n8n`命令行工具已正确安装并可在环境中执行。
- 函数中使用了`tempfile.NamedTemporaryFile`创建临时文件，调用者不需要管理这个临时文件的生命周期，因为它会在用完后自动删除。
- 函数执行中涉及到外部命令的执行，因此需要考虑执行环境的安全性和权限问题。
- 如果替换表达式的执行结果为空或执行过程中出现错误，函数将抛出`BaseException`异常，调用者需要妥善处理这些异常情况。

**输出示例**：
假设`data_item`为`{"name": "Alice", "age": 25}`，`expression`为`"=name & ' is ' & age & ' years old'"`，那么函数可能返回的结果为：
```
"Alice is 25 years old"
```
如果`expression`不以等号开头，例如`expression`为`"Hello World"`，则函数直接返回：
```
"Hello World"
```
***
# FunctionDef replace_exp_recursive
**replace_exp_recursive函数**: 此函数的功能是递归地替换字典中的表达式为对应的值。

replace_exp_recursive函数接收两个字典参数：`data_item`和`expression_dict`。`data_item`字典包含数据项，而`expression_dict`字典包含需要被替换的表达式及其对应值。

函数的工作流程如下：
1. 创建一个新的字典`return_dict`，用于存储替换后的结果。
2. 遍历`expression_dict`字典中的所有键。
3. 对于每个键，获取其对应的表达式值`expression_value`。
4. 如果`expression_value`是一个字典，那么递归调用replace_exp_recursive函数，并将结果存储在`return_dict`中对应的键下。
5. 如果`expression_value`是一个字符串，那么调用`replace_single_exp`函数（该函数未在代码中给出，但可以假设它用于替换单个表达式），并将结果存储在`return_dict`中对应的键下。
6. 返回`return_dict`字典，其中包含了所有替换后的表达式。

在项目中，replace_exp_recursive函数被调用于`replace_exp`函数中，用于处理一个列表`input_data`中的每个数据项。对于`input_data`列表中的每个`data_item`，replace_exp_recursive函数将`expression_dict`中定义的表达式替换为相应的值，并将结果存储在一个新列表`return_list`中，最终返回这个列表。

**注意**：
- 在使用replace_exp_recursive函数时，需要确保`expression_dict`中的表达式与`data_item`中的数据项相匹配，以便正确替换。
- 由于函数使用了递归，应注意避免在`expression_dict`中创建循环引用，这可能导致无限递归。
- 函数假设`expression_dict`中的值只能是字典或字符串，如果存在其他类型，可能需要额外的处理逻辑。

**输出示例**：
假设有以下输入：
```python
data_item = {
    "name": "Alice",
    "age": 30
}
expression_dict = {
    "greeting": "Hello, {{name}}!",
    "age_statement": "You are {{age}} years old."
}
```
调用`replace_exp_recursive(data_item, expression_dict)`后，可能得到的返回值为：
```python
{
    "greeting": "Hello, Alice!",
    "age_statement": "You are 30 years old."
}
```
在这个示例中，`{{name}}`和`{{age}}`被替换为`data_item`中对应的值。
***
# FunctionDef replace_exp
**replace_exp函数**: 此函数的功能是替换输入数据中的json表达式。

replace_exp函数接收两个参数：一个是包含输入数据的列表（input_data），另一个是包含json表达式的字典（expression_dict）。函数的目的是遍历输入数据列表中的每一项，使用另一个函数replace_exp_recursive来递归地替换其中的json表达式，然后将替换后的结果添加到一个新的列表中，并最终返回这个新列表。

详细代码分析如下：
1. 函数定义了一个空列表return_list，用于存储处理后的数据。
2. 使用for循环遍历input_data列表中的每一项数据（data_item）。
3. 对每一项数据调用replace_exp_recursive函数，将data_item和expression_dict作为参数传递。这个函数的作用是递归地替换data_item中的json表达式，具体实现细节未在此代码段中给出。
4. 将replace_exp_recursive函数返回的结果（return_single_dict）添加到return_list列表中。
5. 循环结束后，返回return_list列表，这个列表包含了所有替换后的数据项。

**注意**：
- 使用此函数前，需要确保expression_dict中包含了所有需要替换的json表达式及其对应的替换值。
- 此函数依赖于replace_exp_recursive函数，因此需要确保replace_exp_recursive函数已正确实现且可用。
- 输入数据的格式应为列表，其中每个元素可以是任何数据结构，但replace_exp_recursive函数需要能够处理这些数据结构。

**输出示例**：
假设输入数据为：
```python
input_data = [
    {"name": "{{user_name}}", "age": "{{user_age}}"},
    {"country": "{{user_country}}"}
]
```
表达式字典为：
```python
expression_dict = {
    "{{user_name}}": "张三",
    "{{user_age}}": "30",
    "{{user_country}}": "中国"
}
```
调用replace_exp函数后，可能的返回值为：
```python
[
    {"name": "张三", "age": "30"},
    {"country": "中国"}
]
```
在这个示例中，输入数据列表中的每个字典元素的json表达式都被替换为expression_dict中对应的值。
***
# FunctionDef fill_return_data
**fill_return_data 函数**: 此函数的功能是生成给定输出数据列表的格式化返回数据。

`fill_return_data` 函数接受一个输出数据列表作为参数，并返回一个格式化后的字符串。这个函数的主要作用是将工作流或者其他处理过程中产生的输出数据进行格式化，以便于后续的处理或展示。

函数定义如下：
```python
def fill_return_data(output_data: list) -> str:
    """
    Generate the function comment for the given function body in a markdown code block with the correct language syntax.

    Args:
        output_data (list): 输出数据列表。

    Returns:
        str: 格式化后的返回数据。
    """
    return format_return_data(output_data)
```

在这个函数中，它调用了另一个名为 `format_return_data` 的函数，但是在给定的代码片段中并没有提供 `format_return_data` 函数的实现。我们可以推测，`format_return_data` 函数负责具体的数据格式化逻辑。

在项目中，`fill_return_data` 函数被 `run_pseudo_workflow` 函数调用，用于处理伪节点工作流的输出数据。`run_pseudo_workflow` 函数根据不同的节点类型执行相应的操作，并收集输出数据。最终，它使用 `fill_return_data` 函数来格式化这些输出数据，并将格式化后的数据作为字符串返回。

调用情况如下：
```python
final_return_data = fill_return_data(return_list)
```

**注意**：
- 在使用 `fill_return_data` 函数时，需要确保传入的 `output_data` 参数是一个列表，且 `format_return_data` 函数能够正确处理这个列表。
- 如果 `format_return_data` 函数的实现依赖于特定的数据格式或者结构，那么在调用 `fill_return_data` 之前，需要确保 `output_data` 符合这些要求。

**输出示例**：
假设 `format_return_data` 函数将输出数据列表转换为以逗号分隔的字符串，那么 `fill_return_data` 函数的返回值可能看起来像这样：
```python
"数据1, 数据2, 数据3"
```

这个示例仅仅是一个假设，实际的输出格式将取决于 `format_return_data` 函数的具体实现。
***
