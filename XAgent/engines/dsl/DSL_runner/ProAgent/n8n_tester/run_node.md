# ClassDef n8nRunningException
**n8nRunningException类的功能**：该类用于封装报错类型，并可以重载自定义的错误类型。它包含以下方法：

- `__init__(self, message)`：初始化方法，接收一个错误信息作为参数，并调用父类的初始化方法。该方法还初始化了code_stack和error_message属性。

- `add_context_stack(self, code_context)`：将代码上下文添加到代码堆栈中。接收一个代码上下文作为参数，将其添加到code_stack属性中。

**注意**：n8nRunningException类用于封装报错类型，并提供了添加代码上下文的方法。可以通过调用add_context_stack方法将代码上下文添加到代码堆栈中。

该对象在以下文件中被调用：

- 文件路径：XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/run_code.py
- 相关代码如下：

```python
class n8nNodeRunner():
    def __init__(self, node: n8nPythonNode, name_space: dict):
        self.node = node
        self.name_space: dict = name_space

    def __call__(self, input_data):
        """
        执行具有给定输入数据的函数，并返回输出数据。
        
        参数：
            input_data：要由函数处理的输入数据。
        
        返回：
            函数生成的输出数据。
        
        引发：
            n8nRunningException：如果函数执行过程中发生错误。
        """
        # ...
        try:
            # ...
        except Exception as e:
            # ...
            error = n8nRunningException(e)
            error.error_message = f"{type(e).__name__}: " + str(e)
            self.node.last_runtime_info.runtime_status = RunTimeStatus.ErrorRaisedHere

        assert error != None
        code_split = node_code.split("\n")
        for k, line in enumerate(code_split):
            if ".run" in line:
                line_no = k + 1
                break

        error_codes = ["--> "+code_split[line_no - 1]]
        if line_no < len(code_split) and code_split[line_no].strip() != "":
            error_codes.append( "    "+ code_split[line_no])
        if line_no - 2 >= 0 and code_split[line_no - 2].strip() != "":
            error_codes = ["    "+ code_split[line_no - 2]] + error_codes

        error_codes = [f"In Function: transparent_{self.node.node_meta.node_type.name}"] + error_codes
        error.add_context_stack(error_codes)

        raise error
```

- 文件路径：XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/run_code.py
- 相关代码如下：

```python
def __call__(self, input_data):
    """
    执行具有给定输入数据的工作流程。
    
    参数：
        input_data：要传递给工作流程的输入数据。
    
    返回：
        工作流程生成的输出数据。
    
    引发：
        n8nRunningException：如果执行工作流程过程中发生错误。
    """
    # ...
    try:
        # ...
    except Exception as e:
        # ...
        if isinstance(e, n8nRunningException):
            error = e
            self.workflow.last_runtime_info.runtime_status = RunTimeStatus.ErrorRaisedInner
        else:
            error = n8nRunningException(e)
            error.error_message = f"{type(e).__name__}: " + str(e)
            self.workflow.last_runtime_info.runtime_status = RunTimeStatus.ErrorRaisedHere

    assert error != None
    code_split = workflow_code.split("\n")
    frame = tb[-1]
    counter = 2
    for cont in tb:
        if cont.filename == "<string>":
            counter -= 1
        if counter == 0:
            frame = cont
            break

    if frame.lineno > 0:
        error_codes = ["--> "+code_split[frame.lineno - 1]]
        if frame.lineno < len(code_split) and code_split[frame.lineno].strip() != "":
            error_codes.append( "    "+ code_split[frame.lineno])
        if frame.lineno - 2 >= 0 and code_split[frame.lineno - 2].strip() != "":
            error_codes = ["    "+ code_split[frame.lineno - 2]] + error_codes
        error_codes = [f"In Function: {frame.name}"] + error_codes
        error.add_context_stack(error_codes)

    local_var_info = "注意：如果错误消息中出现'KeyError'，可能是由于输出数据的使用错误。输出数据信息可能对您有所帮助：\n[输出数据信息]\n"
    for action_name in self.name_space.keys():
        if action_name not in ['mainWorkflow', 'mainWorkflow_input_data', '__builtins__'] and 'input_data' not in action_name:
            if self.name_space[action_name] and "node" in self.name_space[action_name].__dict__.keys():
                if self.name_space[action_name].node and "last_runtime_info" in self.name_space[action_name].node.__dict__.keys():
                    if self.name_space[action_name].node.last_runtime_info and "output_data" in self.name_space[action_name].node.last_runtime_info.__dict__.keys():
                        action_output_data = self.name_space[action_name].node.last_runtime_info.output_data
                local_var_info += f"函数`{action_name}`的输出数据为：`{action_output_data}`\n"

    error.add_context_stack([local_var_info])
    raise error
```

- 文件路径：XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/run_code.py
- 相关代码如下：

```python
def run_code(self):
    """
    1. 初始化所有函数的运行时信息。
    2. 执行当前代码并修改所有访问节点的信息。
    """
    # ...
    try:
        # ...
    except n8nRunningException as e:
        # ...
        for code_lines in reversed(e.code_stack):
            self.error_stack_str.extend(code_lines)
            self.error_stack_str.append("------------------------")
        self.error_stack_str.append(e.error_message)
    self.error_stack_str = "\n".join(self.error_stack_str)
```

- 文件路径：XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/run_node.py
- 相关代码如下：

```python
def run(self, input_data, params):
    """
    使用给定的输入数据和参数运行函数。

    参数：
        input_data：函数的输入数据。
        params：函数的参数。

    返回：
        函数的输出数据。

    引发：
        n8nRunningException：如果运行函数时发生错误。
    """
    # ...
    output_data, error = run_node(node=self.node, input_data=input_data)
    if error != "":
        my_error = n8nRunningException(error)
        raise my_error
    else:
        return output_data
```

请注意：
- 以上内容中不包含Markdown的标题和分割线语法。
- 文档主要使用中文编写，如果需要，可以在分析和描述中使用一些英文单词以提高文档的可读性，因为不需要将函数名或变量名翻译为目标语言。
## FunctionDef add_context_stack
**add_context_stack函数**: 这个函数的作用是将代码上下文添加到代码堆栈中。

该函数接受一个参数code_context，表示要添加到堆栈中的代码上下文。

该函数没有返回值。

在调用该函数时，会将code_context添加到code_stack中。

调用该函数的代码位于XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/run_code.py文件中的n8nNodeRunner类的__call__方法中。

该方法用于执行给定输入数据的函数，并返回生成的输出数据。

在调用该函数之前，会更新节点的运行时信息，包括访问次数、测试数据类型和输入数据。

然后，根据节点的名称生成输入数据和输出数据的变量名。

接下来，将输入数据和一个匿名函数添加到name_space中。

然后，根据节点的实现代码生成节点代码。

在执行节点代码时，会捕获可能发生的异常，并将异常信息存储在error对象中。

如果发生异常，会根据异常的堆栈信息找到出错的代码行，并将其添加到error对象的代码堆栈中。

最后，抛出error对象。

需要注意的是：
- 该函数用于将代码上下文添加到代码堆栈中，以便在发生异常时提供更详细的错误信息。
- 在调用该函数之前，会更新节点的运行时信息，包括访问次数、测试数据类型和输入数据。
- 在执行节点代码时，会捕获可能发生的异常，并将异常信息存储在error对象中。
- 如果发生异常，会根据异常的堆栈信息找到出错的代码行，并将其添加到error对象的代码堆栈中。
***
# ClassDef anonymous_class
**anonymous_class 功能**: 该类的功能是执行与 n8nPythonNode 节点相关的函数，并处理输入数据和参数。

anonymous_class 是一个没有具体名称的类，用于封装与 n8nPythonNode 节点相关的函数执行逻辑。它的构造函数接收一个 n8nPythonNode 实例，并将其存储为类的一个属性。这允许类的其他方法能够访问和操作这个节点。

该类定义了一个名为 `run` 的方法，该方法负责使用给定的输入数据和参数来运行函数。方法首先设置一个断点，允许调试器在此处暂停执行，以便开发者可以检查程序状态。然后，它调用一个名为 `run_node` 的函数，该函数负责实际执行与节点相关的逻辑，并返回输出数据和错误信息。

如果 `run_node` 函数返回的错误信息不为空，`run` 方法将创建一个 `n8nRunningException` 异常，并将其抛出。这意味着在执行节点时发生了错误，异常将被上层调用者捕获和处理。如果没有错误，`run` 方法将返回输出数据。

在项目中，`anonymous_class` 被用于 `n8nNodeRunner` 类中。在 `n8nNodeRunner` 的 `__call__` 方法中，通过 `partial` 函数将 `anonymous_class` 与特定的节点实例绑定，并将其存储在命名空间字典中。这样，当执行节点代码时，可以通过 `transparent_{self.node.node_meta.node_type.name}` 来调用 `anonymous_class` 的 `run` 方法。

**注意**:
- 在使用 `anonymous_class` 时，需要确保传递给 `run` 方法的 `input_data` 和 `params` 参数是正确的，以避免运行时错误。
- `run_node` 函数和 `n8nRunningException` 异常需要在上下文中定义，否则 `anonymous_class` 将无法正常工作。
- 由于 `run` 方法中使用了 `pdb.set_trace()`，在非调试环境中使用时应该移除或注释掉这一行。

**输出示例**:
假设 `run_node` 函数正确执行并返回了输出数据 `{"result": "success"}` 和空字符串表示的错误信息，那么 `anonymous_class` 的 `run` 方法将返回以下数据：
```python
{"result": "success"}
```
如果 `run_node` 函数返回了错误信息 `"Error occurred"`，则 `run` 方法将抛出一个 `n8nRunningException` 异常，异常信息将包含 `"Error occurred"`。
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化一个新的实例对象。

详细代码分析与描述：
这段代码定义了一个名为 `__init__` 的函数，它是一个特殊的方法，通常称为类的构造器。当创建类的新实例时，会自动调用这个方法。在这个 `__init__` 方法中，它接收一个参数 `node`，该参数被期望是一个 `n8nPythonNode` 类型的对象。此外，该方法还可以接收任意数量的位置参数 (`*args`) 和关键字参数 (`**kwargs`)。

函数的主体只包含一行代码：
```python
self.node = node
```
这行代码将传入的 `node` 参数赋值给实例的 `node` 属性。这样，创建的实例将持有一个对 `n8nPythonNode` 对象的引用，可以在类的其他方法中使用这个 `node` 对象。

**注意**：
- 在使用这个类创建实例时，必须传入一个 `n8nPythonNode` 类型的对象作为参数。
- 由于 `*args` 和 `**kwargs` 被包含在参数列表中，这个构造器也支持接收额外的位置参数和关键字参数，这些参数在当前函数体中没有被直接使用，但可能会在继承该类的子类中用到或者传递给其他方法。
## FunctionDef run
**run函数**: 该函数的功能是执行给定输入数据和参数的函数。

该`run`函数是一个成员方法，它接收两个参数：`input_data`和`params`。`input_data`是函数的输入数据，其类型可以是任意类型，取决于具体的使用场景。`params`是一个字典，包含了执行函数所需的参数。

在函数的实现中，首先使用了Python的调试器`pdb`，通过`import pdb; pdb.set_trace()`插入了一个断点。这允许开发者在执行到这一行代码时暂停执行，进入调试模式，从而可以检查程序的状态、变量的值等。

接下来，函数调用了`run_node`方法，传入了`self.node`和`input_data`作为参数。`self.node`是该方法所属对象的一个属性，表示当前的节点。`run_node`方法的作用是根据节点和输入数据来执行相应的功能，并返回输出数据和错误信息。

如果`run_node`方法返回的错误信息不为空，即`error != ""`，则会创建一个`n8nRunningException`异常实例，并将错误信息传递给它，然后抛出这个异常。这意味着在执行节点的过程中遇到了错误，需要通过异常处理机制来处理。

如果没有错误发生，`run`函数则会返回`run_node`方法得到的输出数据`output_data`。

**注意**：
- 在使用`run`函数时，需要确保`self.node`已经被正确设置，因为它将决定`run_node`方法的执行逻辑。
- 由于使用了`pdb.set_trace()`，在实际部署或运行代码时应当移除这一调试语句，以避免程序在生产环境中暂停执行。
- 当`run`函数抛出`n8nRunningException`异常时，调用者需要准备好相应的异常处理逻辑。

**输出示例**：
假设`run_node`方法成功执行并返回了一个字符串"Hello, World!"作为输出数据，且没有错误信息，那么`run`函数的返回值将会是：
```python
"Hello, World!"
```
如果`run_node`方法执行过程中遇到了错误，例如错误信息为"Node execution failed"，那么`run`函数将会抛出一个`n8nRunningException`异常。
***
# FunctionDef _get_constant_workflow
**_get_constant_workflow函数**: 这个函数的功能是根据提供的输入数据生成一个常量工作流。

该函数接受一个参数input_data，表示要在工作流中使用的输入数据。返回一个字典，表示生成的工作流。

函数内部首先生成一个触发节点(node_trigger)的信息，包括节点的id、名称、类型等。然后生成一个代码节点(node_code)，其中包含一个jsCode参数，用于将输入数据转换为JSON格式的字符串。接下来生成一个变量节点(node_var)，用于存储工作流的输出结果。

然后，根据节点之间的连接关系，生成一个workflow_connection字典，表示节点之间的连接关系。最后，将节点信息和连接关系整合到workflow_nodes列表中，并生成一个workflow字典，表示完整的工作流。

最后，返回生成的工作流。

**注意**: 使用该代码时需要注意以下几点:
- 输入数据的格式应符合要求，否则可能导致节点执行失败。
- 生成的工作流中的节点类型和参数需要根据实际情况进行调整。

**输出示例**:
```
{
    "versionId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "Simple Workflow",
    "nodes": [
        {
            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "name": "Execute Workflow Trigger",
            "type": "n8n-nodes-base.executeWorkflowTrigger",
            "typeVersion": 1,
            "position": [0, 0],
            "parameters": {}
        },
        {
            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "name": "Code",
            "type": "n8n-nodes-base.code",
            "typeVersion": 2,
            "position": [180, 0],
            "parameters": {
                "jsCode": "return {\"input_data\": \"xxx\"}"
            }
        },
        {
            "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "name": "node_var",
            "position": [360, 0]
        }
    ],
    "connections": {
        "Execute Workflow Trigger": {
            "main": [
                [
                    {
                        "node": "Code",
                        "type": "main",
                        "index": 0
                    }
                ]
            ]
        },
        "Code": {
            "main": [
                [
                    {
                        "node": "node_var",
                        "type": "main",
                        "index": 0
                    }
                ]
            ]
        }
    },
    "active": false,
    "settings": {
        "executionOrder": "v1"
    },
    "tags": []
}
```
***
# FunctionDef run_node
**run_node函数**: 该函数的功能是执行指定的n8n节点。

run_node函数用于执行一个特定的n8n节点，并返回节点执行的状态和相关信息。该函数接收一个n8nPythonNode对象和一个可选的输入数据列表，默认为空字典列表。执行过程中，函数会根据提供的节点信息和输入数据构建一个工作流，然后调用n8n的执行命令来运行该节点，并捕获执行的输出和错误信息。

具体来说，函数首先通过`_get_constant_workflow`函数获取一个常量工作流，并设置工作流ID。然后，它会根据节点的元数据信息设置节点类型、凭证、参数等。如果输入数据中包含`json`键，则会将其值与节点参数合并。函数还会根据不同的集成名称（如slack或googleSheets）设置特定的参数。

如果节点是一个伪节点（pseudoNode），则会尝试运行一个伪工作流并捕获输出；否则，会将工作流写入一个临时文件，并使用n8n的命令行工具执行该文件。执行后，函数会打印输出和错误信息，并根据输出内容判断执行是否成功。

最后，函数会解析输出数据，并返回一个包含输出数据和错误信息的元组。

**注意**:
- 在使用该函数时，需要确保传入的n8nPythonNode对象和输入数据格式正确。
- 如果输入数据为空或格式不正确，可能会导致节点执行失败。
- 函数中使用了subprocess模块来调用n8n命令行工具，因此需要确保n8n已正确安装在系统中，并且可以通过命令行访问。
- 函数中的错误处理依赖于输出中特定的提示字符串（如success_prompt和error_prompt），这要求n8n的输出格式保持一致。

**输出示例**:
```python
# 假设执行成功，没有错误信息
output_data = [{"name": "Alice", "age": 30}]
error = ""

# 假设执行失败，有错误信息
output_data = []
error = "执行节点时发生错误"
```

在项目中调用run_node函数的情况如下：
在`XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/run_node.py`文件中，定义了一个`run`方法，该方法使用`run_node`函数执行一个节点，并处理执行结果。如果执行过程中出现错误，则会抛出一个`n8nRunningException`异常；如果执行成功，则返回输出数据。
***
