# ClassDef MockInput
**MockInput 类的功能**：该类的功能是管理和提供模拟输入数据，用于在测试环境中模拟外部集成的输入。

MockInput 类是一个用于管理和提供模拟输入数据的工具，主要用于在没有实际外部服务调用的情况下测试 n8n 节点（n8nPythonNode）。该类允许开发者在本地存储和检索模拟数据，以便在自动化测试或开发过程中使用。

构造函数 `__init__` 接受两个参数：`mock_pair_dir` 指定存储模拟数据对的目录，默认为 `"./ProAgent/n8n_tester/mock_pairs"`；`persist_on_destruction` 指示对象销毁时是否持久化模拟数据，默认为 `False`。在初始化过程中，该类会注册一个退出时的回调函数 `_flash`，用于在需要时持久化数据。

`_flash` 方法负责在对象销毁时将 `self.mock_data` 字典中的数据持久化到文件系统。该方法会遍历 `self.mock_data` 字典，并将每个集成的数据以 JSON 格式写入到 `self.mock_pair_dir` 目录下对应的文件中。

`get_node_example_input` 方法用于生成给定 n8nPythonNode 的示例输入。它接受一个目标节点和一个可选的 `top_k` 参数，该参数指定生成示例输入的数量，默认为 2。该方法返回一个包含目标节点示例输入的列表。

`register_node_example_input` 方法用于为给定的目标节点注册一个示例输入。它接受一个目标节点和一个目标输入，然后将该输入添加到 `self.mock_data` 字典中对应的位置。

在项目中，`MockInput` 类被用于测试和运行代码的场景。例如，在 `mock_input_test.py` 文件中，它被用于设置测试用例，通过初始化 `mock_interface` 属性为 `MockInput` 对象来模拟输入数据。在 `run_code.py` 文件中，它被用于初始化一个 `MockInput` 对象，以便在代码运行时提供模拟输入。

**注意**：
- 在使用 `MockInput` 类时，需要确保 `mock_pair_dir` 指定的目录存在，并且包含了正确格式的 JSON 文件。
- 如果设置了 `persist_on_destruction` 为 `True`，则在对象销毁时会自动将模拟数据持久化到文件系统中。
- 在调用 `get_node_example_input` 方法时，需要确保传入的 `target_node` 是有效的 n8nPythonNode 实例，并且其 `node_meta` 属性包含了正确的集成名称、资源名称和操作名称。

**输出示例**：
假设我们有一个名为 "test_integration" 的集成，其中包含资源 "test_resource" 和操作 "test_operation"，并且我们已经为其注册了模拟输入数据。当我们调用 `get_node_example_input` 方法时，可能会返回如下的示例输入列表：
```python
[
    {
        "input_field_1": "example_value_1",
        "input_field_2": "example_value_2"
    },
    {
        "input_field_1": "example_value_3",
        "input_field_2": "example_value_4"
    }
]
```
这个列表包含了两个示例输入，每个输入都是一个字典，包含了模拟的输入字段和值。
## FunctionDef __init__
**__init__函数**: 该函数的作用是初始化MockInput类的实例。

该函数接收两个参数：`mock_pair_dir`和`persist_on_destruction`。`mock_pair_dir`参数指定了存储模拟数据对的目录路径，默认值为"./ProAgent/n8n_tester/mock_pairs"。`persist_on_destruction`参数是一个布尔标志，用于指示在对象销毁时是否持久化模拟数据，默认值为False。

在函数体内，首先将`mock_pair_dir`参数赋值给实例变量`self.mock_pair_dir`。然后，使用`atexit.register`函数注册了一个名为`self._flash`的函数，该函数将在程序退出时被调用。接着，将`persist_on_destruction`参数赋值给实例变量`self.persist_on_destruction`。

接下来，初始化了一个名为`self.mock_data`的多层嵌套字典，使用`defaultdict`来确保在访问不存在的键时能自动创建默认值。这里默认值是一个空字典。

然后，函数遍历了`self.mock_pair_dir`目录下的所有文件，使用`os.listdir`获取目录中的文件列表，并使用`tqdm`来显示进度条。对于每个文件名，首先断言它以".json"结尾，然后解析出集成名称`integration_name`。之后，打开并读取该JSON文件，将其内容加载到`json_data_for_integration`变量中。

如果`self.mock_data`中还没有对应的集成名称键，则创建一个空字典作为其值。然后，遍历加载的JSON数据，对于每个资源名称和操作名称，如果在`self.mock_data`中还没有相应的键，则创建一个空列表作为其值。最后，将数据对添加到对应的操作名称列表中。

**注意**:
- 在使用该类时，需要确保`mock_pair_dir`参数指定的目录存在，并且包含了正确格式的JSON文件。
- 如果设置`persist_on_destruction`为True，则需要确保在类实例销毁时调用的`self._flash`函数能够正确地持久化数据。
- 该类的设计使用了`defaultdict`来简化数据结构的初始化和数据访问，但在使用时需要注意，任何对`self.mock_data`的访问都可能导致自动创建新的字典或列表，这可能会影响程序的内存使用。
## FunctionDef _flash
**_flash 函数**: 此函数的功能是在对象销毁时持久化模拟输入数据。

_flash 函数是一个私有方法，它的主要作用是在对象销毁时（如果`self.persist_on_destruction`标志为True），将模拟输入数据（存储在`self.mock_data`字典中）持久化到磁盘。这个过程是通过遍历`self.mock_data`字典中的每一项，并将它们分别写入到`self.mock_pair_dir`目录下对应集成名称的JSON文件中来实现的。

具体来说，函数会检查`self.persist_on_destruction`标志，如果该标志为False，则函数不执行任何操作。如果为True，则会打印一条消息"persisting mock input dictionary"，表示开始持久化过程。然后，它会遍历`self.mock_data`字典中的每一项，对于每个集成（integration），它会尝试打开一个以集成名称命名的JSON文件，并将对应的值（val）以JSON格式写入文件。如果在写入过程中发生异常，它会捕获异常并打印出来。

在项目中，_flash 函数被注册为`atexit`模块的一个回调函数，这意味着当程序退出时，_flash 函数会被自动调用。这是在`mock_input.py`文件的`__init__`方法中完成的，其中`atexit.register(self._flash)`这行代码就是注册过程。此外，`__init__`方法还负责初始化`self.mock_pair_dir`和`self.persist_on_destruction`属性，并从磁盘加载现有的模拟输入数据。

**注意**：
- `_flash`函数是一个内部使用的私有方法，不应该直接从类的外部调用。
- 在使用这个函数之前，确保`self.mock_data`已经正确初始化，并且包含了要持久化的数据。
- 如果`self.persist_on_destruction`为False，函数将不会执行任何操作。
- 持久化过程中可能会遇到文件写入错误或权限问题，需要确保`self.mock_pair_dir`目录是可写的。

**输出示例**：由于此函数没有返回值，它不会产生直接的输出。但在持久化过程中，如果成功，它会在`self.mock_pair_dir`目录下创建或更新对应集成名称的JSON文件。如果发生错误，它会在控制台打印出异常信息。
## FunctionDef get_node_example_input
**get_node_example_input函数**: 该函数的功能是生成给定n8nPythonNode的示例输入。

`get_node_example_input`函数接收一个`n8nPythonNode`对象和一个可选的整数`top_k`作为参数。`n8nPythonNode`是一个节点对象，它包含了节点的元数据，例如集成名称、资源名称和操作名称。`top_k`参数指定了要生成的示例输入的数量，默认值为2。

函数的主要工作是根据传入的`target_node`节点对象，从`mock_data`字典中检索与该节点相关的模拟数据。`mock_data`是一个嵌套的字典结构，它按照集成名称、资源名称和操作名称来组织数据。函数返回一个列表，其中包含了为目标节点生成的示例输入。

在项目中，`get_node_example_input`函数被以下文件调用：

1. `mock_input_test.py`：在这个测试文件中，`get_node_example_input`函数被用于获取特定节点的模拟输入数据，以便进行断言测试。测试确保函数返回的数据与预期的模拟数据一致。

2. `run_code.py`：在这个运行文件中，`get_node_example_input`函数被用于获取触发器节点的示例输入数据。这些数据随后被用于初始化触发器节点的运行时信息，并作为触发器节点的输入数据。

**注意**：
- 在使用`get_node_example_input`函数时，需要确保`mock_data`字典已经被正确初始化，并且包含了目标节点所需的模拟数据。
- 如果`mock_data`字典中不存在对应节点的模拟数据，函数可能会返回空列表或者抛出错误。

**输出示例**：
假设有一个n8nPythonNode对象`node`，其对应的模拟数据为`[{"id": 1, "name": "Sample Data"}]`，那么调用`get_node_example_input(node)`可能会返回以下结果：
```python
[{"id": 1, "name": "Sample Data"}]
```
这个列表包含了为目标节点`node`生成的示例输入数据。
## FunctionDef register_node_example_input
**register_node_example_input函数**：此函数的功能是为特定的目标节点注册一个示例输入。

此函数`register_node_example_input`用于向模拟数据中的特定节点注册一个示例输入。它接受两个参数：`target_node`和`target_input`。`target_node`是一个`n8nPythonNode`对象，代表需要注册示例输入的目标节点。`target_input`是一个表示示例输入的数据，可以是字典、列表或其他数据结构，取决于节点期望的输入格式。

函数内部首先从`target_node`对象中提取出`integration_name`（集成名称）、`resource_name`（资源名称）和`operation_name`（操作名称）。这三个值用于定位模拟数据字典中的具体位置，以便将示例输入添加到正确的节点下。

接着，函数检查对应位置的数据是否为`[{}]`（即一个包含一个空字典的列表），这是模拟数据的初始状态。如果是，它会将该位置的数据重置为空列表，以便添加新的示例输入。

最后，函数将`target_input`添加到模拟数据字典的相应位置。这样，当测试或模拟该节点的行为时，就可以使用这些注册的示例输入。

在项目中，此函数被调用的情况如下所示：
文件路径为`XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/mock_input_test.py`，在该文件中的`test_register_mock_input`方法中，创建了一个测试用的节点`mock_node`，并通过调用`register_node_example_input`方法，为该节点注册了一个示例输入`target_input`。然后，使用断言测试确保通过`get_node_example_input`方法能够正确地检索到之前注册的示例输入。

**注意**：
- 使用此函数时，需要确保`target_node`对象已经正确初始化，并且包含了有效的`integration_name`、`resource_name`和`operation_name`属性。
- 在注册示例输入之前，应该检查`mock_data`字典是否已经初始化到足够深的层级，以避免出现键错误。
- 此函数不返回任何值，它的主要作用是修改`mock_data`字典，以便在后续的测试或模拟中使用。
- 在测试框架中，应该在每个测试用例之后清理或重置`mock_data`，以避免不同测试用例之间的数据干扰。
***
