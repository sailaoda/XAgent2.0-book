# ClassDef DSLAction
**DSLAction函数**: 这个类的函数是DSLAction。

DSLAction类是一个用于DSL编译器的动作类。它包含了DSL编译器中的动作类型、动作名称、人类可读名称、对象名称、配置参数等属性。它还包含了动作的代码、运行时信息、输入输出动作等。

该类具有以下函数和属性：

- `__hash__()`函数：用于计算对象的哈希值。
- `__eq__(other)`函数：用于判断两个DSLAction对象是否相等。
- `been_visited`属性：用于判断该动作是否已被访问过。
- `to_code()`函数：将动作转换为代码形式。
- `gen_code()`函数：生成动作的代码。
- `been_visited_by_data_edge`属性：存储已被数据边访问过的节点集合。
- `been_visited_by_break_edge`属性：存储已被中断边访问过的节点集合。

注意：在使用DSLAction类时，需要注意配置参数的设置和代码生成的过程。

**输出示例**：
```python
class DSLAction():
    action_type: DSLActionType
    action_name: str = ""
    human_name: str = ""
    object_name: str = ""
    config_params: dict = {}

    code: List[str] = []

    last_runtime_info: TestResult = TestResult()

    out_actions: list[list[Any]] = []
    in_actions: list[list[Any]] = []

    been_visited_by_data_edge: Set[str] = set()
    been_visited_by_break_edge: Set[str] = set()

    def __hash__(self):
        return self.object_name.__hash__()

    def __eq__(self, other):
        if isinstance(other, DSLAction):
            return self.object_name == other.object_name
        return False

    @property
    def been_visited(self):
        return len(self.been_visited_by_break_edge) + len(self.been_visited_by_data_edge) > 0

    def to_code(self):
        if self.code != []:
            return self.code
        self.code = self.gen_code()
        return self.code

    def gen_code(self):
        param_str = json.dumps(self.config_params, indent=2, ensure_ascii=False)
        param_str = param_str.splitlines(True)
        param_str = [line.strip("\n") for line in param_str]
        prefix = "config_params = "
        param_str[0] = prefix + param_str[0]
        param_str = [code_indent + line for line in param_str]

        lines = [
            f"def {self.object_name}(tool_params: dict, tool_input_data=None):",
            f"{code_indent}# {self.human_name}",
            f"{code_indent}temp_func = transparent_function(tool_type=\"{self.action_type.name}\",tool_name=\"{self.action_name}\")",
            *param_str,
            f"{code_indent}tool_params.update(config_params)",
            f"{code_indent}tool_output = temp_func.run(tool_params, tool_input_data)",
            f"{code_indent}global_vars[\"tool_result_chain\"].append(tool_output)",
            f"{code_indent}global_vars[\"tool_result_dict\"][\"{self.object_name}\"].append(tool_output)",
            f"{code_indent}return tool_output"
        ]

        return lines
```
## FunctionDef been_visited
**been_visited函数**: 该函数的功能是判断一个动作节点是否被访问过。

在DSL编译器的上下文中，`been_visited`函数用于检查一个动作节点（action node）是否至少被一个break边或者data边访问过。这个函数通过检查两个成员变量`been_visited_by_break_edge`和`been_visited_by_data_edge`的长度之和是否大于0来判断。如果任一变量包含元素，则表示该动作节点已被访问过。

在`compile_DSL.py`文件中，`been_visited`函数被用于编译过程中，以确定哪些动作节点需要生成代码。在初始化全局变量和生成函数代码的过程中，只有被访问过的动作节点才会被考虑。这样可以避免生成未使用节点的代码，从而优化最终的工作流代码。

例如，在`init_global_var`方法中，`been_visited`函数用于过滤掉未被访问的动作节点，只为被访问过的动作节点在全局变量字典中创建空列表。

在`compile_workflow`函数中，`been_visited`函数同样用于过滤，只为被访问过的动作节点生成代码。如果一个动作节点未被访问过，编译器会记录一条警告信息。

**注意**：
- 在使用`been_visited`函数时，需要确保`been_visited_by_break_edge`和`been_visited_by_data_edge`两个成员变量已经正确地更新，以反映动作节点的访问状态。
- 该函数不接受任何参数，并且返回一个布尔值，表示动作节点是否被访问过。

**输出示例**：
假设一个动作节点已经被一个break边访问过，但未被任何data边访问过，那么`been_visited_by_break_edge`的长度为1，`been_visited_by_data_edge`的长度为0，`been_visited`函数将返回`True`。如果两个变量都为空集合，那么`been_visited`函数将返回`False`。
***
# ClassDef DSLEdge
**DSLEdge 类的功能**：DSLEdge 类用于表示 DSL 动作之间的边。它包含了边的名称、类型、注释等属性，并提供了将边转换为代码的方法。

代码分析和描述：
- edge_name: str 类型的属性，表示边的名称。
- edge_type: DSLEdgeType 类型的属性，表示边的类型，可以是 DataEdge 或者 BreakEdge。
- comments: List[str] 类型的属性，表示边的注释信息。
- route: Optional[Callable] 类型的属性，表示选边时的路由函数。
- param_sufficient: bool 类型的属性，表示是否需要提供足够的参数。
- code: List[str] 类型的属性，表示将边转换为代码后的结果。

to_code(self, to_node: DSLAction) 方法用于将边转换为代码。如果已经存在转换后的代码，则直接返回。否则，根据边的属性生成相应的代码。具体的代码生成逻辑如下：
- 首先判断是否存在注释，如果存在则将注释添加到代码中。
- 如果存在路由函数，则获取路由函数的代码，并将其添加到代码中。
- 根据边的类型，生成相应的代码逻辑。如果是 DataEdge，则生成调用 AI_complete_params 函数的代码，并将结果赋值给相应的变量。如果是 BreakEdge，则生成断点的代码。
- 最后将生成的代码保存到 code 属性中，并返回结果。

注意事项：
- DSLEdge 类的实例用于表示 DSL 动作之间的边，可以通过设置不同的属性来控制边的行为。
- 在使用 to_code 方法之前，需要先设置好边的属性，以便正确生成相应的代码。

输出示例：
```python
code = [
    'if not route_edge_edge_name_result.param_sufficient:',
    '    route_edge_edge_name_result = AI_complete_params_name(',
    '        provided_params=route_edge_edge_name_result,',
    '        comments=[',
    '            "comment1",',
    '            "comment2",',
    '        ]',
    '    )',
    '    assert route_edge_edge_name_result.param_sufficient',
    'params_for_to_node = route_edge_edge_name_result.provide_params',
]
```
## FunctionDef to_code
**to_code函数**: 此函数的功能是生成特定DSLAction对象的代码。

此函数`to_code`是一个用于将DSL（Domain Specific Language）中的动作转换为可执行代码的方法。它是DSL编译器的一部分，用于处理单个工具（tool）的选边逻辑，并生成相应的代码片段。

函数接受一个参数`to_node`，这是一个`DSLAction`类型的对象，代表了DSL中的一个动作节点。

函数的主要逻辑如下：

1. 如果当前对象的`code`属性不为空，则直接返回该属性，这意味着代码已经生成过了，无需重复生成。

2. 初始化一个空的代码行列表`lines`和注释列表`comments`。

3. 如果当前对象有注释，则将注释格式化并添加到`comments`列表中。

4. 根据当前对象是否有路由（`route`）来决定代码生成的逻辑：
   - 如果有路由，则获取路由函数的纯代码（`get_pure_func_code(self.route)`），并将其添加到代码行列表中。接着，生成一系列代码行来处理参数的完整性，并为目标节点提供参数。
   - 如果没有路由，则生成代码行来直接调用AI补全参数的函数，并为目标节点提供参数。

5. 最后，将生成的代码行列表赋值给当前对象的`code`属性，并返回该列表。

在项目中的调用情况如下：

在`compile_DSL.py`文件中，`to_code`函数被用于访问DSL中的动作节点，并生成相应的代码。在处理普通动作节点时，如果动作节点的边（edge）没有足够的参数，则会调用`to_code`函数来生成补全参数的代码。如果边已经有足够的参数，则会生成一个空的参数字典，并调用相应的工具函数。

**注意**：
- 在使用`to_code`函数时，需要确保传入的`to_node`对象是正确配置的`DSLAction`实例。
- 生成的代码依赖于外部的`name_allocator`和`AI_complete_params_`等函数或对象，因此在使用此函数之前，需要确保这些依赖项已经正确设置。

**输出示例**：
```python
[
    "if not route_edge_example_result.param_sufficient:",
    "    route_edge_example_result = AI_complete_params_example(",
    "        provided_params = route_edge_example_result,",
    "        # 这里是注释",
    "    )",
    "    assert route_edge_example_result.param_sufficient",
    "params_for_example = route_edge_example_result.provide_params",
]
```
以上示例展示了当存在路由且参数不足时，`to_code`函数可能生成的代码片段。
***
# ClassDef DSLSwitch
**DSLSwitch函数**：这个类的功能是定义了一个DSL切换动作，用于在DSL编译器中进行路由切换。

该类继承自DSLAction类，表示DSL中的一个切换动作。切换动作可以对应一个或多个路由函数。该类具有以下属性和方法：

- action_type: 表示动作类型，这里固定为DSLActionType.Switch。
- _uuid: 表示动作的唯一标识符。
- route: 表示路由函数，可以为空。
- object_name: 表示动作的对象名称。

该类重写了父类的`__hash__`和`__eq__`方法，用于判断两个DSLSwitch对象是否相等。

该类还定义了一个`uuid`属性，用于生成动作的唯一标识符。

该类还定义了一个`gen_code`方法，用于生成DSL编译后的代码。如果存在路由函数，则不生成代码；否则，生成AI路由的代码。

**注意**：在使用该类时，需要注意以下几点：
- 可以通过设置`route`属性来指定路由函数。
- 生成的代码中，如果存在路由函数，则不会生成代码；否则，会生成AI路由的代码。

**输出示例**：
```python
def switch() -> DSLRouteResult:
    # Reason for choice 0, routing to tool(action_name):
    #     comment_1,
    #     comment_2,
    #     ...
    # AI-RouteAgent will perform a switch and provide tool_params based on above reasons
    route_result = AI_route_and_give_params_1234567890()
    assert route_result.param_sufficient == True
    return route_result
```
## FunctionDef uuid
**uuid函数**: 这个函数的功能是生成一个唯一的标识符。

该函数用于生成一个唯一的标识符。如果当前的标识符为None，则调用name_allocator.assign_gid方法为该对象分配一个全局唯一的标识符，并将其赋值给_uuid属性。最后返回该标识符。

**注意**: 在使用该函数时需要注意以下几点:
- 如果_uuid属性已经被赋值，则不会再次生成新的标识符，而是直接返回已有的标识符。
- 生成的标识符是全局唯一的，可以用于唯一标识一个对象。

**输出示例**:
```
"e3e3b6a2-4a8f-4f3e-9d9e-2d5e5e5e5e5e"
```
## FunctionDef gen_code
**gen_code函数**: 该函数的功能是生成代码行列表，这些代码行代表了一个动态路由决策的函数体。

该函数接受一个名为`action_dict`的参数，默认为空字典。函数的主要逻辑分为两部分：

1. 如果存在`self.route`属性，即存在路由节点（route_node），则函数暂时不执行任何操作（使用`pass`语句）。

2. 如果不存在`self.route`属性，即不存在路由节点，函数将生成一个名为`self.object_name`的函数定义，这个函数返回`DSLRouteResult`类型的结果。函数体内部包含了对输出动作（`self.out_actions`）的遍历，为每个动作生成注释说明，并调用一个AI路由函数（`AI_route_and_give_params_`），该函数名通过`name_allocator.assign_ai_route(self)`动态分配。最后，函数断言路由结果的参数是否充足（`param_sufficient == True`），并返回路由结果。

具体代码分析如下：

- 首先检查`self.route`是否为`None`。如果不为`None`，意味着存在预定义的路由节点，此时函数不执行任何操作。
- 如果`self.route`为`None`，则进入AI路由逻辑。首先定义一个函数，函数名为`self.object_name`，返回类型为`DSLRouteResult`。
- 接着，遍历`self.out_actions`，这是一个包含边（edge）和动作（action）的元组列表。对于每个动作，生成注释说明，包括选择的原因和路由到特定工具的说明。
- 然后，添加一个注释，说明AI路由代理将根据上述原因执行切换，并提供工具参数。
- 调用一个AI路由函数，该函数名通过`name_allocator`动态生成，以确保每次调用都有唯一的函数名。
- 断言路由结果的参数是否充足，如果不充足，将触发断言错误。
- 最后，返回路由结果。

**注意**：
- 在实际使用中，需要确保`self.out_actions`已经被正确初始化，且包含了所有可能的输出动作。
- `name_allocator`应该是一个能够生成唯一函数名的实例，以避免函数名冲突。
- 断言语句用于确保AI路由的结果是有效的，如果`param_sufficient`不为`True`，则会抛出异常。

**输出示例**：
```python
[
    "def example_function() -> DSLRouteResult:",
    "    # Reason for choice 0, routing to tool(tool_name):",
    "    #     This is a comment explaining the reason",
    "    # AI-RouteAgent will perform a switch and provide tool_params based on above reasons",
    "    route_result = AI_route_and_give_params_unique_id()",
    "    assert route_result.param_sufficient == True",
    "    return route_result",
]
```
以上示例展示了如果`self.object_name`为`example_function`，并且存在一个输出动作，其工具名为`tool_name`，注释为"This is a comment explaining the reason"，那么生成的代码行列表将如上所示。
***
# ClassDef DSLWhile
**DSLWhile类的功能**：DSLWhile类代表了DSL（Domain Specific Language）中的while循环结构，用于在DSL编译过程中处理循环逻辑。

DSLWhile类是DSLAction类的子类，它特定于表示while循环结构。在DSL中，while循环是一种控制流结构，它允许代码重复执行，直到满足某个条件为止。DSLWhile类在DSL编译器中用于识别和处理这种循环结构。

类的定义中包含了一个类属性`action_type`，它被设置为`DSLActionType.While`，这表明该类代表的是while循环结构。

DSLWhile类还重写了`__hash__`方法和`__eq__`方法。`__hash__`方法返回对象名称的哈希值，这使得DSLWhile实例可以用作哈希表中的键。`__eq__`方法用于比较两个DSLWhile实例是否相等，它通过比较对象名称来判断两个实例是否代表同一个while循环。

在项目中，DSLWhile类的实例是在`compile_DSL.py`文件的`load_action`函数中创建的。当解析到JSON数据中的节点类型为while循环时，会创建一个DSLWhile实例，并分配一个唯一的对象名称。这个对象名称是通过`name_allocator.assign_gid(None)`函数生成的唯一标识符，然后与"while_"前缀组合而成。

**注意**：
- 在使用DSLWhile类时，需要确保其对象名称的唯一性，因为这会影响到实例的哈希值和等价性比较。
- DSLWhile类的实例应该只在处理DSL中的while循环结构时使用。

**输出示例**：
由于DSLWhile类主要用于内部逻辑的表示，并不直接产生可视化的输出，因此没有具体的输出示例。但在内部逻辑中，一个DSLWhile实例可能会被创建如下：

```python
while_node = DSLWhile(
    action_type=DSLActionType.While,
    action_name="while",
    human_name="循环节点",
    object_name="while_1"  # 假设name_allocator.assign_gid(None)返回的是1
)
```

在这个示例中，`while_node`是一个DSLWhile实例，代表了一个名为"循环节点"的while循环结构，其内部唯一标识符为"while_1"。
## FunctionDef __hash__
**__hash__函数**: 该函数的作用是为对象提供一个哈希值。

在Python中，`__hash__`方法用于获取对象的哈希值，通常用于字典键值对的键或者集合中的元素，以支持快速的成员检查。当一个对象需要被哈希时（例如，当你将对象添加到一个集合中），Python会调用这个方法。

在这段代码中，`__hash__`方法被定义为返回`self.object_name`属性的哈希值。这意味着对象的哈希值直接依赖于其`object_name`属性的哈希值。这样设计的目的可能是因为`object_name`是对象的唯一标识符，或者因为它包含了决定对象哈希值的所有必要信息。

**注意**:
- 确保`self.object_name`是不可变的，因为哈希值应该在对象的整个生命周期内保持不变。
- 如果`self.object_name`的值可以改变，那么对象的哈希值也会改变，这可能会导致在使用哈希表（如字典或集合）时出现问题。
- 如果对象需要作为字典的键或集合的成员，那么它必须是可哈希的，即它必须实现`__hash__`方法，并且还应该实现`__eq__`方法以支持比较操作。

**输出示例**:
假设`self.object_name`的值为"example_object"，那么调用`__hash__`方法可能会返回如下的哈希值：

```python
hash_value = example_instance.__hash__()
print(hash_value)  # 输出可能是一个整数，例如: 123456789
```

这个哈希值是一个整数，它代表了对象在内存中的唯一位置，可以用于快速比较和检索操作。
## FunctionDef __eq__
**__eq__ 函数**: 该函数的功能是判断两个DSLWhile对象是否相等。

该函数`__eq__`是一个特殊方法，用于比较两个对象是否相等。在Python中，当使用`==`运算符比较两个对象时，会自动调用该对象的`__eq__`方法。

在这段代码中，`__eq__`方法首先检查`other`参数是否是一个`DSLWhile`类型的实例。如果是，那么它会进一步比较当前对象（即`self`）的`object_name`属性和`other`对象的`object_name`属性是否相同。如果这两个属性值相等，那么这两个`DSLWhile`对象被认为是相等的，函数返回`True`。如果`other`不是`DSLWhile`类型的实例，或者`object_name`属性不相等，那么函数返回`False`。

**注意**：
- 使用这个`__eq__`方法时，需要确保比较的对象都有`object_name`这个属性。
- 这个方法只能用于比较`DSLWhile`类型的对象，如果尝试与其他类型的对象比较，将会返回`False`。

**输出示例**：
假设有两个`DSLWhile`对象，它们的`object_name`属性分别为`"loop1"`和`"loop1"`，那么比较这两个对象：

```python
dsl_while1 = DSLWhile("loop1")
dsl_while2 = DSLWhile("loop1")
result = dsl_while1 == dsl_while2  # 调用__eq__方法
```

在这个例子中，`result`的值将会是`True`，因为两个对象的`object_name`属性相同。
***
# ClassDef DSLMeta
**DSLMeta 类的功能**：该类用于定义和处理DSL（领域特定语言）元数据，包括名称、目的、作者、输入参数和输出参数。

DSLMeta 类是一个简单的数据结构，用于存储和转换与DSL相关的元数据。它包含以下属性：
- `name`：字符串类型，表示DSL的名称。
- `purpose`：字符串类型，表示DSL的目的或用途。
- `author`：字符串类型，表示DSL的作者。
- `input_params`：字典类型，表示DSL的输入参数及其类型。
- `output_params`：字典类型，表示DSL的输出参数及其类型。

该类提供了两个方法：
1. `from_json`：这是一个静态方法，用于从JSON格式的元数据中创建一个DSLMeta实例。它接受一个包含元数据的字典，并返回一个新的DSLMeta实例。
2. `to_json`：这个方法用于将DSLMeta实例的属性转换为一个字典，便于将元数据转换为JSON格式。注意，此方法只转换了部分属性，没有包括`output_params`。

在项目中，DSLMeta 类被用于编译DSL时，从JSON格式的元数据中提取信息，并将这些信息存储在DSL对象中。例如，在`compile_DSL.py`文件中，`compile_DSL_from_pipeline`函数使用`DSLMeta.from_json`方法来创建一个包含元数据的DSL对象，并将其存储在`out_DSL.global_var`中。

**注意**：
- 在使用`from_json`方法时，需要确保传入的字典包含所有必要的键，否则会抛出`KeyError`。
- `to_json`方法并不返回所有属性，如果需要完整的元数据信息，可能需要修改此方法以包含`output_params`。

**输出示例**：
```python
meta_data = {
    "name": "ExampleDSL",
    "purpose": "To demonstrate DSLMeta usage",
    "author": "Developer",
    "input_params": {"param1": "int", "param2": "str"},
    "output_params": {"result": "bool"}
}

dsl_meta_instance = DSLMeta.from_json(meta_data)
print(dsl_meta_instance.to_json())
```
可能的输出结果为：
```json
{
    "name": "ExampleDSL",
    "purpose": "To demonstrate DSLMeta usage",
    "author": "Developer",
    "input_params": {"param1": "int", "param2": "str"}
}
```
请注意，输出中不包含`output_params`字段。如果需要包含此字段，需要对`to_json`方法进行相应的修改。
## FunctionDef from_json
**from_json函数**: 该函数的功能是将包含DSL元数据的JSON对象转换为DSLMeta实例。

该`from_json`函数是一个用于解析JSON格式的元数据并创建一个DSLMeta对象的工具函数。它接收一个参数`meta_data`，这个参数应该是一个字典，包含了必要的DSL元数据信息，如名称、目的、作者以及输入输出参数等。

具体来说，函数内部通过访问`meta_data`字典中的键值来获取相应的元数据信息，并将这些信息作为参数传递给DSLMeta类的构造函数，从而创建一个新的DSLMeta对象。创建的DSLMeta对象包含以下属性：

- `name`: DSL的名称。
- `purpose`: DSL的目的或描述。
- `author`: DSL的作者。
- `input_params`: DSL的输入参数列表。
- `output_params`: DSL的输出参数列表。

在项目中，`from_json`函数被用于`compile_DSL.py`文件中的`compile_DSL_from_pipeline`函数里。在这个上下文中，它用于读取一个包含DSL元数据的JSON文件，并将这些元数据转换为DSLMeta对象，这个对象随后被用于构建DSL对象的全局变量部分。

**注意**：在使用`from_json`函数时，需要确保传入的`meta_data`字典包含所有必要的键值对，否则在尝试访问不存在的键时会抛出`KeyError`异常。

**输出示例**:
假设我们有以下的JSON格式的元数据：

```json
{
  "name": "ExampleDSL",
  "purpose": "To demonstrate the from_json function",
  "author": "Developer",
  "input_params": ["param1", "param2"],
  "output_params": ["result"]
}
```

调用`from_json`函数并传入上述JSON对象，将返回一个DSLMeta实例，其属性如下：

```python
DSLMeta(
  name="ExampleDSL",
  purpose="To demonstrate the from_json function",
  author="Developer",
  input_params=["param1", "param2"],
  output_params=["result"]
)
```
## FunctionDef to_json
**to_json函数**: 该函数的功能是将DSL对象的元数据转换为JSON格式的字典。

该`to_json`函数是一个实例方法，它的作用是将DSL对象的一些属性序列化为一个字典，以便可以将其转换为JSON格式。这样做通常是为了便于存储或传输。在这个项目中，`to_json`函数被用来序列化DSL对象的元数据，这些元数据包括：

- `name`：DSL对象的名称。
- `purpose`：DSL对象的目的或用途。
- `author`：创建或定义DSL对象的作者。
- `input_params`：DSL对象接受的输入参数列表。

在`compile_DSL.py`文件中，`to_json`函数被用来获取`DSL`类的实例`out_DSL`的全局变量`global_var`的JSON表示。这个表示可能会用于进一步的处理或作为编译过程的一部分来存储DSL的元数据。

**注意**：
- 在使用`to_json`函数时，需要确保调用它的DSL对象实例已经正确地设置了所有需要序列化的属性。
- 该函数不接受任何参数，并且返回一个字典，这意味着它的返回值可以直接被`json`模块的`dump`或`dumps`方法使用来生成JSON字符串。

**输出示例**：
调用`to_json`函数可能会返回如下形式的字典：

```json
{
    "name": "ExampleDSL",
    "purpose": "To demonstrate JSON serialization",
    "author": "Developer",
    "input_params": ["param1", "param2", "param3"]
}
```

这个字典随后可以被转换为JSON字符串，例如：

```json
{
    "name": "ExampleDSL",
    "purpose": "To demonstrate JSON serialization",
    "author": "Developer",
    "input_params": ["param1", "param2", "param3"]
}
```

这样的JSON字符串可以用于存储或在网络中传输DSL对象的元数据。
***
# ClassDef DSL
**DSL类功能**：DSL类是一个用于解析和执行DSL（领域特定语言）的类。它包含了DSL的动作列表、边列表和全局变量等属性。DSL类的主要功能是根据DSL的定义，生成相应的代码，并执行该代码。

DSL类具有以下主要属性和方法：

- `action_list`：DSL动作列表，存储了DSL中定义的所有动作。
- `edge_list`：DSL边列表，存储了DSL中定义的所有边。
- `global_var`：全局变量，存储了DSL中定义的全局变量。
- `codes`：生成的DSL代码。
- `last_runtime_info`：最后一次运行时的信息。

DSL类具有以下主要方法：

- `init_global_var()`：初始化全局变量。
- `to_code()`：将DSL对象转换为代码。
- `visit_action()`：根据DSL的动作类型，生成相应的代码块。
- `visit_switch()`：生成Switch类型动作的代码块。
- `visit_while()`：生成While类型动作的代码块。
- `compile_workflow()`：编译DSL并生成DSL工作流代码。
- `run_DSL()`：执行DSL代码。

**注意**：DSL类用于解析和执行DSL代码。在使用DSL类时，需要先调用`compile_workflow()`方法编译DSL，并使用`run_DSL()`方法执行DSL代码。

DSL类的代码解析和说明：
DSL类是用于解析和执行DSL代码的类。它包含了DSL的动作列表、边列表和全局变量等属性。DSL类的主要功能是根据DSL的定义，生成相应的代码，并执行该代码。

DSL类的`init_global_var()`方法用于初始化全局变量。它会将全局变量中的`tool_result_chain`和`tool_result_dict`初始化为空列表和空字典，并遍历动作列表，将未访问过的动作添加到`tool_result_dict`中。

DSL类的`to_code()`方法用于将DSL对象转换为代码。它会遍历动作列表，根据动作的类型生成相应的代码块，并将其添加到代码列表中。

DSL类的`visit_action()`方法用于根据DSL的动作类型生成相应的代码块。对于普通动作，它会根据边的参数是否充足生成相应的代码。对于Switch类型动作，它会生成Switch语句的代码块，并根据选择的分支执行相应的代码。对于While类型动作，它会生成While循环的代码块，并根据循环条件执行相应的代码。

DSL类的`visit_switch()`方法用于生成Switch类型动作的代码块。它会根据选择的分支生成相应的代码，并执行相应的动作。

DSL类的`visit_while()`方法用于生成While类型动作的代码块。它会生成While循环的代码块，并根据循环条件执行相应的代码。

DSL类的`compile_workflow()`方法用于编译DSL并生成DSL工作流代码。它会遍历动作列表，根据动作的类型生成相应的代码块，并将其添加到工作流代码中。

DSL类的`run_DSL()`方法用于执行DSL代码。它会先调用`compile_workflow()`方法编译DSL代码，然后根据输入数据执行DSL代码，并记录运行时的信息。

**注意**：在使用DSL类时，需要先调用`compile_workflow()`方法编译DSL，并使用`run_DSL()`方法执行DSL代码。
## FunctionDef init_global_var
**init_global_var 函数**: 该函数的功能是初始化全局变量。

该函数`init_global_var`是一个成员方法，它负责初始化存储工具执行结果的全局变量。具体来说，它会创建两个全局变量：`tool_result_chain`和`tool_result_dict`。`tool_result_chain`是一个列表，用于按顺序存储工具的执行结果；`tool_result_dict`是一个字典，用于存储以工具名称为键，工具执行结果列表为值的映射。

在函数内部，它遍历成员变量`self.action_list`中的所有动作（action）。对于每个动作，如果动作类型是`Start`、`End`或`While`，则跳过这个动作，因为这些类型的动作不产生工具执行结果。此外，如果动作尚未被访问（`been_visited`属性为`False`），也会跳过，因为未被访问的动作同样不会产生结果。

对于其他类型的动作，函数会在`tool_result_dict`字典中为每个动作的`object_name`（即工具名称）初始化一个空列表，这个列表将用来存放该工具的执行结果。

在项目中，`init_global_var`函数被`compile_workflow`函数调用。`compile_workflow`函数位于`compile_DSL.py`文件中，它的作用是编译DSL（Domain Specific Language）工作流代码。在编译过程中，需要初始化全局变量，以便在生成的工作流代码中使用。`init_global_var`函数在编译过程的特定阶段被调用，以确保在工作流代码中使用的全局变量是最新初始化的状态。

**注意**：
- 在调用`init_global_var`函数之前，应确保`self.action_list`已经被正确填充，且每个动作的`been_visited`属性已经根据需要被设置。
- 由于`init_global_var`函数会修改全局变量，因此在并发或多线程环境中使用时需要特别注意同步和线程安全的问题。
- 在调用此函数后，应避免直接修改`tool_result_chain`和`tool_result_dict`，除非你清楚这样做的后果，因为这可能会影响工作流代码的执行逻辑。
***
