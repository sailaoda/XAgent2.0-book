# ClassDef PythonNotebook
**PythonNotebook函数**：这个类的功能是提供一个Python笔记本环境，用于运行Python代码。

该类继承自BaseEnv类，具有以下方法：

- `__init__(self, config: Dict[str, Any] = None)`: 初始化方法，接受一个配置字典作为参数。在初始化过程中，会根据配置字典设置工作空间和笔记本配置，并创建一个新的笔记本对象和笔记本客户端对象。

- `_running(self) -> bool`: 检查笔记本客户端是否正在运行。如果笔记本客户端的内核连接存在，则返回True，否则返回False。

- `_reset(self)`: 重置笔记本客户端。如果笔记本客户端正在运行，则先清理内核连接，然后创建一个新的内核管理器和内核客户端对象。

- `_fix_escape(problematic_code: str) -> str`: 修复代码中的转义字符。该方法会检查代码中的字符串，并将其中的换行符、回车符和制表符替换为转义字符。

- `execute_cell(self, code: str, cell_index: int = None, reset: bool = False) -> str`: 执行笔记本中的代码单元格。该方法接受一个代码字符串作为参数，并可选择指定要执行的单元格索引和是否在执行前重置内核。执行完毕后，将输出保存到笔记本对象中，并返回执行结果。

- `print_notebook(self) -> str`: 打印笔记本中所有单元格的内容和输出。该方法遍历笔记本对象的所有单元格，将代码和输出格式化后返回。

- `_format_outputs(self, outputs, cell_index=None, reraise=False, return_binary=False)`: 格式化输出结果。该方法接受一个输出列表作为参数，并可选择指定单元格索引、是否重新引发异常和是否返回二进制数据。根据输出的类型和数据格式，将输出结果格式化为不同的形式。

- `_format_output(self, output, cell_index=None, reraise=False, return_binary=False)`: 格式化单个输出结果。该方法接受一个输出对象作为参数，并可选择指定单元格索引、是否重新引发异常和是否返回二进制数据。根据输出的类型和数据格式，将输出结果格式化为不同的形式。

**注意**：在使用`execute_cell`方法执行代码时，需要确保提供的代码是有效的Python代码，并且不要在代码中使用以`!`开头的shell命令。

**输出示例**：模拟代码返回值的可能外观。

请注意：
- 生成的内容中不应包含Markdown的标题和分隔符语法。
- 主要使用中文编写，如果需要，可以在分析和描述中使用一些英文单词，以增强文档的可读性，因为不需要将函数名或变量名翻译为目标语言。
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化 Python 编程环境对象。

该函数是 Python 编程环境对象的构造函数，用于初始化该对象的状态。它接受一个可选的配置字典 `config` 作为参数，该参数包含了初始化环境所需的配置信息。

详细代码分析如下：

1. `super().__init__(config)`：这行代码调用了父类的构造函数，将配置字典 `config` 传递给父类进行初始化。

2. `self.workspace = self.config['workspace']`：从配置字典中提取 `'workspace'` 键对应的值，并将其赋值给对象的 `workspace` 属性。这个属性通常用于指定工作空间的路径。

3. `self.nb_cfg = self.config['notebook']`：从配置字典中提取 `'notebook'` 键对应的值，并将其赋值给对象的 `nb_cfg` 属性。这个属性包含了与 notebook 相关的配置信息。

4. `if not os.path.exists(self.workspace): os.mkdir(self.workspace, mode=0o777)`：这段代码检查 `workspace` 指定的路径是否存在，如果不存在，则创建一个新的目录，并设置权限模式为 `0o777`，即任何用户都有读、写、执行的权限。

5. `self.nb = nbformat.v4.new_notebook(metadata={'kernelspec': {'name': 'python', 'language': 'python', 'display_name': 'python'}})`：使用 `nbformat` 库创建一个新的 notebook 对象，并设置其元数据，指定使用的内核为 Python。

6. `self.nbc = NotebookClient(self.nb, timeout=self.nb_cfg['timeout'])`：创建一个 `NotebookClient` 对象，它将用于与 notebook 交互。`NotebookClient` 对象使用之前创建的 notebook 对象，并从配置中获取 `timeout` 参数。

**注意**：
- 在使用这段代码之前，需要确保传入的配置字典 `config` 包含了 `'workspace'` 和 `'notebook'` 这两个键，并且它们对应的值是正确的，否则在初始化过程中可能会抛出 `KeyError`。
- 创建工作空间目录时，设置了较宽松的权限模式 `0o777`，这在多用户环境下可能会引起安全问题，因此在实际部署时应根据实际情况调整权限设置。
- `NotebookClient` 对象的创建依赖于 `nbformat` 库和 `notebook` 库，因此在使用前需要确保这些依赖已经正确安装在运行环境中。
## AsyncFunctionDef _running
**_running函数**: 该函数的作用是检查Python代码执行环境是否存活。

该函数是一个异步函数，其主要功能是判断当前的Python代码执行环境（通常是一个Jupyter内核）是否处于活跃状态。如果内核客户端（`self.nbc.kc`）存在，则调用其`_async_is_alive`方法来异步检查内核是否存活。如果内核客户端不存在，则直接返回`False`，表示内核不处于活跃状态。

在`_running`函数中，`self.nbc.kc`是一个内核客户端对象，它提供了与Jupyter内核进行交互的接口。`_async_is_alive`方法是一个异步方法，它返回一个布尔值，表示内核是否存活。

在项目中，`_running`函数被调用于两个场景：
1. 在`_reset`函数中，用于在重置内核之前检查内核是否存活。如果内核存活，则先进行清理操作，然后再创建新的内核管理器并启动新的内核。
2. 在`execute_cell`函数中，用于在执行代码单元之前检查内核是否存活。如果内核不存活或者需要重置内核，则会调用`_reset`函数来重置内核。

**注意**：
- 由于`_running`函数是一个异步函数，因此在调用时需要使用`await`关键字。
- 在使用`_running`函数时，需要确保它被包含在一个异步环境中，例如在`async def`定义的异步函数中调用。
- `_running`函数不接受任何参数，并且返回一个布尔值，表示内核是否存活。

**输出示例**：
- 如果内核客户端存在且内核存活，`_running`函数可能返回`True`。
- 如果内核客户端不存在或内核不存活，`_running`函数将返回`False`。
## AsyncFunctionDef _reset
**_reset 函数**: 该函数的功能是重置 Python 代码执行环境。

_reset 函数是一个异步函数，它的主要作用是在执行 Python 代码之前重置代码执行环境，确保代码在一个干净的环境中运行。这个函数在 ToolServerNode 的 Python 编程环境模块中定义，用于管理 Jupyter Notebook 内核的状态。

具体来说，_reset 函数的执行流程如下：
1. 首先，函数会检查当前是否有正在运行的内核。如果有，它会调用 `self.nbc._async_cleanup_kernel()` 来异步清理当前的内核。
2. 然后，函数会调用 `self.nbc.create_kernel_manager()` 来创建一个新的内核管理器。
3. 接着，函数会调用 `self.nbc.async_start_new_kernel(cwd=self.workspace)` 来异步启动一个新的内核，并将当前工作目录设置为 `self.workspace`。
4. 最后，函数会调用 `self.nbc.async_start_new_kernel_client()` 来异步启动一个新的内核客户端。

在 ToolServerNode 的 Python 编程环境模块中，_reset 函数被 `execute_cell` 函数调用。`execute_cell` 函数的作用是创建或替换一个笔记本单元格并执行它，然后返回执行结果。在执行代码之前，如果传入的 `reset` 参数为 `True` 或者当前没有运行的内核，`execute_cell` 函数会调用 `_reset` 函数来重置内核。

**注意**：
- 由于 `_reset` 是一个异步函数，因此在调用时需要使用 `await` 关键字。
- 在调用 `_reset` 函数之前，确保所有需要保存的数据已经被正确处理，因为重置内核会导致当前内核的所有状态丢失。
- `_reset` 函数的调用可能会抛出异常，因此在使用时应当妥善处理可能出现的异常情况。
- 该函数是 ToolServerNode 的内部实现细节，通常不应直接由外部代码调用，而是通过 `execute_cell` 等高级接口间接使用。
## FunctionDef _fix_escape
**_fix_escape函数**: 该函数的功能是修复字符串中的转义字符问题。

该函数接收一个字符串参数`problematic_code`，该字符串可能包含了一些特殊字符，如换行符`\n`、回车符`\r`和制表符`\t`，这些特殊字符在某些情况下可能会导致代码的解析或执行出现问题。`_fix_escape`函数的目的是在不改变字符串原本含义的前提下，将这些特殊字符转换为它们的转义形式，以确保代码字符串在处理时的正确性和稳定性。

函数内部首先定义了一个字符串列表`str_sign`，包含了四种引号符号：双引号`"`、单引号`'`、三个连续的双引号`"""`和三个连续的单引号`'''`。这些符号通常用于定义Python中的字符串。

接下来，函数使用`re.findall`方法和正则表达式`pattern`来查找`problematic_code`中所有被这些引号包围的字符串片段。正则表达式使用了非贪婪匹配`(.*?)`来确保匹配最短的符合条件的字符串，并且通过`re.DOTALL`标志允许`.`匹配包括换行符在内的任意字符。

对于每个找到的字符串片段，函数将其中的换行符`\n`、回车符`\r`和制表符`\t`替换为它们的转义形式`\\n`、`\\r`和`\\t`。这一步骤通过遍历`in_line_strs`列表并对每个元素调用`replace`方法来完成。

最后，函数使用`zip`函数将原始字符串片段和修改后的字符串片段配对，然后在`problematic_code`中将原始字符串替换为修改后的字符串。这样，所有的特殊字符都被正确地转义了。

**注意**：使用该函数时，需要确保传入的字符串`problematic_code`是有效的Python代码字符串，且函数只处理字符串中的特殊字符，不会改变其他代码逻辑。

**输出示例**：
如果`problematic_code`为：
```python
print("Hello\nWorld")
```
则函数返回的`fixed_code`将会是：
```python
print("Hello\\nWorld")
```
在这个例子中，换行符`\n`被转义为`\\n`，从而在字符串中保持了文字上的`\n`，而不是实际的换行效果。
## AsyncFunctionDef execute_cell
**execute_cell函数**: 此函数的功能是创建或替换一个笔记本单元格并执行它，然后返回输出结果。

此函数是一个异步函数，用于在Jupyter笔记本环境中执行Python代码。它允许用户快速测试代码片段，并检查执行结果是否符合预期。

函数接受三个参数：
- `code` (str): 要执行的Python代码字符串。用户需要确保提供的是有效的Python代码，并且格式正确。不应该在这里提供以'!'开头的shell命令。
- `cell_index` (int, 可选): 指定要插入或覆盖代码的单元格索引。默认值为`None`，表示在笔记本的末尾追加新单元格。如果指定了具体的索引，则会覆盖相应索引位置的单元格。
- `reset` (bool, 可选): 表示是否在执行代码前重置内核。默认值为`False`。

函数的主要逻辑如下：
1. 如果指定了重置内核，或者当前内核未运行，则会先重置内核。
2. 根据`cell_index`的值，决定是追加新单元格还是覆盖现有单元格。如果`cell_index`为`None`、等于单元格数量或单元格数量为0，则追加新单元格；否则，覆盖指定索引的单元格。
3. 尝试执行指定的单元格。如果执行过程中遇到`CellExecutionError`或`DeadKernelError`，则会捕获异常并进行相应处理。对于`DeadKernelError`，会再次尝试重置内核。
4. 将笔记本的内容写入到指定的文件路径。
5. 返回执行结果，格式化后的输出。

**注意**：
- 在使用此函数时，需要确保提供的代码是有效的Python代码。
- 如果在执行过程中内核死亡，函数会尝试重置内核。
- 函数返回的是格式化后的执行结果，包括输出的文本和二进制数据。

**输出示例**：
假设执行了以下代码：
```python
code = 'print("hello world")'
```
可能的返回值为：
```python
['cell_index: 0', 'hello world']
```
这表示在索引为0的单元格中执行了打印"hello world"的操作，并成功返回了输出结果。
## FunctionDef print_notebook
**print_notebook 函数**: 此函数的功能是打印笔记本中所有单元格的内容和输出。

该函数`print_notebook`是一个成员方法，它的作用是遍历笔记本（notebook）对象中的所有单元格（cells），并将每个单元格的内容以及相应的输出格式化为字符串。这个方法通常用于获取笔记本中代码和输出的文本表示，便于查看或记录。

详细代码分析如下：
1. 初始化一个空字符串`ret`，用于存储最终的输出结果。
2. 使用`enumerate`函数遍历笔记本对象中的单元格列表`self.nb.cells`，`enumerate`会同时返回单元格的索引（`i`）和单元格对象（`cell`）。
3. 对于每个单元格，首先添加一个标记该单元格索引的标题，格式为“= Cell i =”，其中`i`是单元格的索引。
4. 判断单元格的类型是否为`code`（代码单元格）。如果是，将单元格的源代码（`cell["source"]`）添加到`ret`字符串中。
5. 如果代码单元格有输出（`cell['outputs']`列表的长度不为0），则添加一个输出标题“= Output i =”，并调用`self._format_outputs`方法来格式化输出内容，然后将格式化后的输出添加到`ret`字符串中。
6. 最后，函数返回`ret`字符串，它包含了笔记本中所有代码单元格的内容和相应的输出。

**注意**：
- 在使用此函数时，需要确保`self.nb.cells`是一个包含笔记本单元格信息的列表，且每个单元格对象都有`cell_type`、`source`和`outputs`等关键字段。
- `_format_outputs`方法应该能够正确地格式化单元格的输出内容，这个方法的实现细节在这段代码中没有给出，需要在类的其他部分中定义。

**输出示例**：
```
= Cell 0 =
print("Hello, World!")
= Output 0 =
Hello, World!

= Cell 1 =
x = 10
y = 20
print(x + y)
= Output 1 =
30
```
以上示例展示了如果笔记本中有两个代码单元格，第一个单元格打印了一句问候语，第二个单元格计算了两个数的和，`print_notebook`函数将会返回这样格式化的字符串。
## FunctionDef _format_outputs
**_format_outputs 函数**: 此函数的功能是格式化执行代码单元格后的输出结果。

`_format_outputs` 函数是一个私有方法，用于将代码执行后的输出结果进行格式化处理，以便于更好地展示和理解执行结果。该函数接收以下参数：
- `outputs`: 代码执行后的输出结果列表。
- `cell_index`: 可选参数，表示当前执行的代码单元格索引。
- `reraise`: 可选参数，表示是否在输出中重新抛出异常，默认为`False`。
- `return_binary`: 可选参数，表示返回的结果是否为二进制数据，默认为`False`。

函数的处理逻辑如下：
1. 如果`outputs`列表为空，则根据`cell_index`是否为`None`返回空字符串或者`cell_index`的信息。
2. 如果`outputs`列表只有一个元素，则直接调用`_format_output`对单个输出进行格式化。如果`cell_index`不为`None`，则将格式化后的输出和`cell_index`信息组合成一个字典返回。
3. 如果`outputs`列表有多个元素，则对每个输出都调用`_format_output`进行格式化，并将所有格式化后的输出组合成一个列表，放在一个字典中返回。如果`cell_index`不为`None`，则在列表的开头插入`cell_index`的信息。

在项目中，`_format_outputs`函数被调用于`pycoding.py`文件中的两个地方：
1. `execute_cell`方法中，用于执行代码单元格并返回执行结果。如果执行过程中遇到异常，会通过`reraise=True`参数重新抛出异常，并通过`return_binary=True`参数以二进制形式返回结果。
2. `print_notebook`方法中，用于打印整个笔记本中所有代码单元格的内容和输出结果。在这个方法中，`_format_outputs`用于格式化每个代码单元格的输出结果，以便打印。

**注意**：
- `_format_outputs`函数是一个私有方法，通常不应该直接从类外部调用。
- 函数的返回值取决于传入的`outputs`列表内容，以及`cell_index`、`reraise`和`return_binary`参数的值。

**输出示例**：
假设有以下输出结果列表：
```python
outputs = [
    {'name': 'stdout', 'output_type': 'stream', 'text': 'Hello World\n'},
    {'output_type': 'error', 'ename': 'Exception', 'evalue': 'An error occurred'}
]
```
调用`_format_outputs(outputs, cell_index=1)`可能会返回如下格式的字典：
```python
{
    'type': 'composite',
    'data': [
        'cell_index: 1',
        {'name': 'stdout', 'output_type': 'stream', 'text': 'Hello World\n'},
        {'output_type': 'error', 'ename': 'Exception', 'evalue': 'An error occurred'}
    ]
}
```
## FunctionDef _format_output
**_format_output 函数**: 此函数的作用是格式化执行结果的输出。

_format_output 函数是一个私有方法，用于将代码执行的输出结果进行格式化处理，以便于后续的显示或其他处理。该函数接收四个参数：`output` 是一个包含输出内容的字典；`cell_index` 是一个可选参数，表示输出内容所在的单元格索引；`reraise` 是一个布尔值，用于控制当输出类型为错误时是否重新抛出异常；`return_binary` 是一个布尔值，用于控制返回的二进制数据是否需要包装。

函数内部首先定义了一个辅助函数 `format_single_data`，用于处理单个数据项的格式化。根据数据类型的不同，该辅助函数会返回不同格式的数据。例如，如果数据类型以 'image/' 开头，则返回一个包含二进制数据和媒体类型的字典；如果数据类型以 'text/' 开头，则返回连接后的字符串；如果数据类型以 'application/' 开头，则直接返回数据本身。

_format_output 函数主体使用了模式匹配来处理不同的输出类型。对于 'execute_result' 或 'display_data' 类型的输出，函数会检查输出数据中是否包含 'text/html' 和 'text/plain'，并在必要时移除 'text/html'。然后根据数据键的数量，使用 `format_single_data` 函数进行格式化。对于 'error' 类型的输出，如果 `reraise` 参数为真，则会抛出一个 `ToolExecutionError` 异常；否则，会返回错误的跟踪信息。对于 'stream' 类型的输出，直接返回输出文本。对于其他类型的输出，直接返回原始输出数据。

在项目中，_format_output 函数被 `_format_outputs` 函数调用，用于处理可能包含多个输出结果的情况。`_format_outputs` 函数会遍历所有输出结果，并对每个结果调用 `_format_output` 进行格式化。如果提供了 `cell_index`，则会在格式化后的数据中添加单元格索引信息。

**注意**：在使用 _format_output 函数时，需要确保传入的 `output` 参数格式正确，并且根据实际情况设置 `reraise` 和 `return_binary` 参数。

**输出示例**：
假设 `output` 参数包含以下内容：
```python
{
    'output_type': 'execute_result',
    'data': {
        'text/plain': 'Hello, World!',
        'text/html': '<strong>Hello, World!</strong>'
    }
}
```
调用 `_format_output(output)` 可能返回的结果为：
```python
'Hello, World!'
```
如果 `return_binary` 为真，并且数据类型为 'image/png'，则可能返回的结果为：
```python
{
    'type': 'binary',
    'media_type': 'image/png',
    'data': '`Wrapped`'
}
```
### FunctionDef format_single_data
**format_single_data 函数**: 此函数的功能是根据提供的数据和数据类型格式化单个数据项。

此函数接受两个参数：`data` 和 `data_type`。`data` 是需要被格式化的数据，`data_type` 是一个字符串，表示数据的类型。

函数内部根据 `data_type` 的不同前缀来决定如何格式化数据：
- 如果 `data_type` 以 'image/' 开头，函数会返回一个字典，包含三个键值对：'type' 设置为 'binary'，'media_type' 设置为 `data_type`，以及 'data'。如果外部变量 `return_binary` 为真，则 'data' 键对应的值为原始的 `data`，否则为字符串 '`Wrapped`'。
- 如果 `data_type` 以 'text/' 开头，函数会将 `data` 列表中的所有字符串元素连接起来，然后返回这个连接后的字符串。
- 如果 `data_type` 以 'application/' 开头，函数直接返回 `data`。
- 如果 `data_type` 不符合以上任何一种情况，函数也直接返回 `data`。

在项目中，`format_single_data` 函数被嵌套在 `_format_output` 方法中调用。`_format_output` 方法处理输出数据，根据输出数据的类型（例如执行结果、显示数据、错误或流输出等），使用 `format_single_data` 函数来格式化每个数据项。例如，当输出类型为 'execute_result' 或 'display_data' 时，`_format_output` 会遍历输出数据的所有键，并对每个键对应的数据调用 `format_single_data` 函数进行格式化。

**注意**：
- 在使用此函数时，需要确保 `data_type` 是一个有效的 MIME 类型字符串，以便函数能够正确识别并格式化数据。
- 如果 `data` 是二进制数据，且 `data_type` 以 'image/' 开头，需要根据 `return_binary` 的值决定是否返回原始的二进制数据。
- 此函数设计为内部使用，通常不会直接从外部调用，而是作为 `_format_output` 方法的一部分来使用。

**输出示例**：
假设我们有以下输入数据和数据类型：
```python
data = b'\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR\x00\x00\x00\x01\x00\x00\x00\x01\x08\x06\x00\x00\x00\x1f\x15\xc4\x89'
data_type = 'image/png'
```
如果 `return_binary` 为真，调用 `format_single_data(data, data_type)` 可能返回的结果如下：
```python
{
    'type': 'binary',
    'media_type': 'image/png',
    'data': b'\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR\x00\x00\x00\x01\x00\x00\x00\x01\x08\x06\x00\x00\x00\x1f\x15\xc4\x89'
}
```
如果 `return_binary` 为假，返回的结果将是：
```python
{
    'type': 'binary',
    'media_type': 'image/png',
    'data': '`Wrapped`'
}
```
***
