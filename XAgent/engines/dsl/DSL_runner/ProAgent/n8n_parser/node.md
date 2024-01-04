# ClassDef n8nNodeMeta
**n8nNodeMeta函数**: 这个类的函数是用来表示n8n节点的元数据信息。

该类具有以下属性：
- node_type: 节点类型，枚举类型，可选值为NodeType中的trigger和action。
- integration_name: 集成名称，表示节点所属的集成。
- resource_name: 资源名称，表示节点所操作的资源。
- operation_name: 操作名称，表示节点所执行的操作。
- operation_description: 操作描述，表示节点操作的描述信息。

该类还具有以下方法：
- to_action_string(): 生成节点执行的操作的字符串表示。

**注意**: 
- 该类用于表示n8n节点的元数据信息，可以通过设置属性来描述节点的类型、集成、资源和操作。
- to_action_string()方法用于生成节点执行的操作的字符串表示，可以根据节点的属性生成相应的字符串。
- 可以根据需要设置节点的属性，以便后续使用。

**输出示例**:
```
action(resource=default, operation=default)
```
## FunctionDef to_action_string
**to_action_string函数**: 此函数的功能是生成节点执行动作的字符串表示形式。

该函数`to_action_string`属于某个节点类的实例方法，用于生成并返回该节点所代表的动作的字符串描述。这个描述包括节点类型、资源名称和操作名称。如果节点还有操作描述，那么这个描述也会被包含在生成的字符串中。

具体来说，函数首先使用Python的格式化字符串功能，将节点类型的名称、资源名称和操作名称插入到一个字符串模板中。这个模板是`"{self.node_type.name}(resource={self.resource_name}, operation={self.operation_name})"`，其中`self.node_type.name`、`self.resource_name`和`self.operation_name`分别代表节点类型的名称、资源名称和操作名称。

如果节点的操作描述`self.operation_description`不为空字符串，那么函数会在原有字符串的基础上，通过添加`": {self.operation_description}"`来包含这个描述。最终，函数返回这个完整的字符串。

**注意**：
- 在使用这个函数之前，确保节点实例的`node_type`、`resource_name`、`operation_name`和`operation_description`属性已经被正确地设置，因为这些属性的值将直接影响到返回的字符串内容。
- 这个函数不接受任何参数，并且总是返回一个字符串。

**输出示例**：
假设一个节点的类型名称为`HTTPRequest`，资源名称为`APIResource`，操作名称为`GET`，操作描述为`Fetch user data`，那么`to_action_string`函数的返回值可能看起来像这样：
```
HTTPRequest(resource=APIResource, operation=GET): Fetch user data
```
如果操作描述为空，那么返回值可能看起来像这样：
```
HTTPRequest(resource=APIResource, operation=GET)
```
***
# ClassDef n8nPythonNode
**n8nPythonNode函数**: 这个类的功能是将n8n节点转化为一个Python函数。

该类具有以下方法：

- `get_name()`: 返回表示节点名称的字符串。
- `get_runtime_description()`: 获取工作流的最后一次运行时信息的描述。
- `update_implement_info()`: 更新节点的实现信息。
- `print_self_clean()`: 返回一个多行文本，表示节点的代码。
- `print_self()`: 返回一个多行文本，表示节点的代码。
- `parse_parameters(param_json: dict) -> (ToolCallStatus, str)`: 解析输入参数并检查其是否符合预期格式。
- `parse_display_options(display_options, node: n8nPythonNode) -> bool`: 检查给定的显示选项是否应该解析为给定的n8nPythonNode。
- `parse_properties(node: n8nPythonNode) -> dict`: 解析给定节点的属性，并返回一个包含参数描述的字典。

**注意**: 
- `get_name()`方法返回节点的名称，该名称由节点类型和节点ID的组合构成。
- `get_runtime_description()`方法返回最后一次工作流运行时的描述信息。
- `update_implement_info()`方法用于更新节点的实现信息。
- `print_self_clean()`方法返回一个多行文本，表示节点的代码。如果节点未实现，则在代码中添加注释。
- `print_self()`方法返回一个多行文本，表示节点的代码。如果节点未实现，则在代码中添加注释。
- `parse_parameters(param_json: dict) -> (ToolCallStatus, str)`方法用于解析输入参数并检查其是否符合预期格式。
- `parse_display_options(display_options, node: n8nPythonNode) -> bool`方法用于检查给定的显示选项是否应该解析为给定的n8nPythonNode。
- `parse_properties(node: n8nPythonNode) -> dict`方法用于解析给定节点的属性，并返回一个包含参数描述的字典。

**输出示例**:
```python
node = n8nPythonNode()
print(node.get_name())  # Output: "node_type_1"
print(node.get_runtime_description())  # Output: "This NodeType has not been implemented"
node.update_implement_info()
print(node.implemented)  # Output: False
print(node.print_self_clean())  # Output: ["def node_type_1():", "  params = {", "    \"param1\": {", "      \"name\": \"param1\",", "      \"type\": \"string\",", "      \"value\": \"\",", "      \"required\": True", "    }", "  }", "  function = transparent_node_type(integration=\"integration_name\", resource=\"resource_name\", operation=\"operation_name\")", "  output_data = function.run(input_data=None, params=params)", "  return output_data"]
print(node.print_self())  # Output: ["def node_type_1():", "  \"\"\"", "  comments: ", "  TODOs: ", "    - ", "  \"\"\"", "  params = {", "    \"param1\": {", "      \"name\": \"param1\",", "      \"type\": \"string\",", "      \"value\": \"\",", "      \"required\": True", "    }", "  }", "  function = transparent_node_type(integration=\"integration_name\", resource=\"resource_name\", operation=\"operation_name\")", "  output_data = function.run(input_data=None, params=params)", "  return output_data"]
param_json = {"param1": {"name": "param1", "type": "string", "value": "test", "required": True}}
status, result = node.parse_parameters(param_json)
print(status)  # Output: ToolCallStatus.ToolCallSuccess
print(result)  # Output: {"result": [], "status": "ToolCallSuccess"}
```
## FunctionDef get_name
**get_name函数**：该函数返回一个表示节点名称的字符串。

该函数接受一个参数self，表示节点对象。

该函数返回一个字符串，表示节点的名称，该名称由节点类型和节点ID组合而成。

该函数主要用于获取节点的名称。

**注意**：无

**输出示例**：节点类型为"action"，节点ID为"1"，则返回字符串"action_1"。
## FunctionDef get_runtime_description
**get_runtime_description 函数**: 此函数的功能是获取工作流最后一次运行的信息。

此函数`get_runtime_description`定义在一个节点解析器类中，用于获取与节点相关的最后一次运行时的描述信息。它不接受任何参数，并返回一个字符串类型的值。

函数内部，首先检查实例变量`self.last_runtime_info`的值。这个变量存储了节点最后一次运行的状态信息，它的值来自一个名为`RunTimeStatus`的枚举，这个枚举定义了可能的运行状态。

如果`self.last_runtime_info`的值等于`RunTimeStatus.DidNotImplemented`，这意味着该节点尚未实现。在这种情况下，函数将返回一个格式化的字符串，告知用户该节点类型尚未实现。

函数的返回值是一个字符串，描述了节点最后一次运行的状态。如果节点已经实现并运行过，那么这个描述将包含运行的具体信息；如果节点尚未实现，则会告知用户这一点。

**注意**：在使用此函数时，需要确保`self.last_runtime_info`已经被正确地设置为`RunTimeStatus`枚举中的一个值。否则，函数可能无法正确地返回最后一次运行的描述。

**输出示例**：
如果节点尚未实现，函数可能返回的字符串示例为：
```
"This <节点类型> has not been implemented"
```
其中`<节点类型>`将被替换为实际的节点类型名称。
## FunctionDef update_implement_info
**update_implement_info函数**：此函数的功能是更新实现信息。

该函数用于更新实现信息。如果参数列表为空，则将implemented属性设置为True并返回。否则，遍历参数列表，如果参数的data_is_set属性为True，则将implemented属性设置为True并返回。

**注意**：在使用此代码时需要注意以下几点：
- 该函数用于更新实现信息，可以根据参数列表中的参数是否设置了数据来判断是否已实现。
- 如果参数列表为空，则表示已实现。
- 如果参数列表不为空，则遍历参数列表，如果有任何一个参数的data_is_set属性为True，则表示已实现。

**输出示例**：模拟代码返回值的可能外观。
```python
None
```
## FunctionDef print_self_clean
**print_self_clean函数**: 此函数的功能是生成一个多行文本，描述了一个n8n节点的定义和执行逻辑。

该函数首先创建一个空的列表`lines`，用于存储生成的代码行。接着，根据节点的类型（动作节点或触发器节点），设置`input_data`变量的定义字符串。如果是动作节点，`input_data`将被定义为一个列表包含字典的形式；否则，不需要定义`input_data`。

函数定义行`define_line`使用`get_name`方法获取当前节点的名称，并构造成一个函数定义的字符串，然后添加到`lines`列表中。

接下来，函数遍历节点的参数`params`，并将每个参数转换为JSON格式的字符串。这些参数字符串被整理成多行，并添加到`lines`列表中。如果节点尚未实现，会在参数行中添加注释以指示需要实现的部分，或者表明该函数不需要特定参数。

然后，函数添加了一个调用`transparent_{self.node_meta.node_type.name}`函数的行，这是一个假设存在的函数，用于根据节点的集成名称、资源名称和操作名称执行操作。

根据节点类型，函数将添加不同的执行行。如果是动作节点，将调用`function.run`方法并传入`input_data`和`params`；如果是触发器节点，则`input_data`将传入`None`。

最后，函数添加了一个返回输出数据的行，并将整个`lines`列表作为函数的返回值。

**注意**：
- 在实际使用中，需要根据实际情况填充`transparent_{self.node_meta.node_type.name}`函数的具体实现。
- 生成的代码行是为了方便理解和可读性，实际代码中可能需要根据上下文环境进行适当的修改和调整。
- 该函数假设`params`字典中的每个值都有一个`to_json`方法，用于转换为JSON格式的字符串。

**输出示例**：
```python
[
    "def action_node(input_data: List[Dict] =  [{...}]):",
    "  params = {",
    "    \"param1\": \"value1\",",
    "    \"param2\": \"value2\"",
    "  }  # to be Implemented",
    "  function = transparent_action(integration=\"integration_name\", resource=\"resource_name\", operation=\"operation_name\")",
    "  output_data = function.run(input_data=input_data, params=params)",
    "  return output_data"
]
```
以上示例展示了一个动作节点的定义和执行逻辑的代码行列表。
## FunctionDef print_self
**print_self函数**：print_self函数返回一个多行文本。

该函数的作用是生成一个包含函数定义、注释和参数信息的多行文本。

函数定义行包括函数名和参数信息，如果节点类型是action，则还包括input_data参数的定义。

注释部分包括节点的注释和TODO列表。

参数信息部分将参数转换为json格式，并按照一定的格式进行排列和缩进。

接下来是根据节点类型生成function对象的代码。

如果节点类型是action，则调用function对象的run方法，并传入input_data和params作为参数。

如果节点类型不是action，则调用function对象的run方法，并传入None和params作为参数。

最后返回output_data。

**注意**：在使用该函数时需要注意以下几点：
- 函数的返回值是一个多行文本，可以根据需要进行处理和展示。
- 函数中的参数信息是根据节点的params属性生成的，需要确保params属性的正确性。
- 函数中的function对象是根据节点的node_meta属性生成的，需要确保node_meta属性的正确性。

**输出示例**：
```
def print_self(self):
    define_line = f"def {self.get_name()}({input_data}):"
    comments = f"comments: {self.node_comments}"
    TODOs = ["- todo1", "- todo2"]
    params = {
        "param1": {
            "type": "string",
            "default": "value1",
            "description": "param1的描述"
        },
        "param2": {
            "type": "number",
            "default": 0,
            "description": "param2的描述"
        }
    }
    function = transparent_action(integration="integration_name", resource="resource_name", operation="operation_name")
    output_data = function.run(input_data=input_data, params=params)
    return output_data
```
## FunctionDef parse_parameters
**parse_parameters 函数**: 此函数的功能是解析输入参数并检查它们是否符合预期格式。

此函数接收一个字典类型的参数 `param_json`，该参数包含了需要被解析的输入参数。函数的主要任务是验证这些参数是否符合工具调用所需的格式，并返回一个包含工具调用状态和结果的元组。

函数首先会创建 `self.params` 的深拷贝 `new_params`，并调用每个参数的 `refresh` 方法来重置它们的状态。然后，函数会检查 `param_json` 是否为字典类型，如果不是，则返回一个错误状态和错误信息。

接下来，函数会遍历 `param_json` 中的所有键，并对每个键执行以下检查：
1. 如果键不在 `new_params` 中，则返回未定义参数的错误状态和信息。
2. 如果参数值是空字符串，则返回必需参数未提供的错误状态和信息。
3. 使用 `new_params[key].parse_value` 方法解析参数值，如果解析失败，则返回相应的错误状态和信息。

如果所有参数都成功解析，函数会更新 `self.params` 为新的参数字典 `new_params`，并调用 `self.update_implement_info` 方法更新实现信息。最后，函数返回成功状态和包含解析结果的 JSON 字符串。

在项目中，`parse_parameters` 函数被 `compiler.py` 文件中的 `handle_rewrite_params` 方法调用。在该方法中，首先检查函数名称是否存在，然后尝试将 `tool_input["params"]` 解析为 JSON。如果解析成功，它会调用 `parse_parameters` 函数来处理参数重写，并根据返回的状态决定后续的操作。

**注意**：
- 输入参数 `param_json` 必须是字典类型，否则会引发 `TypeError`。
- 函数返回的状态是 `ToolCallStatus` 枚举类型的一个值，它表示工具调用的状态。
- 函数返回的 JSON 字符串包含了错误信息或解析结果，以及工具调用的状态名称。

**输出示例**：
如果输入参数类型不正确，可能的返回值示例为：
```json
{
    "error": "Parameter Type Error: The parameter is expected to be a json format string which can be parsed as dict type. However, you are giving string parsed as <class 'str'>",
    "result": "Nothing happened.",
    "status": "ParamTypeError"
}
```
如果所有参数都成功解析，可能的返回值示例为：
```json
{
    "result": ["解析后的参数1", "解析后的参数2"],
    "status": "ToolCallSuccess"
}
```
***
