# ClassDef FunctionManager
**FunctionManager函数**: 这个类提供了管理函数的方法，包括函数的注册和执行。函数是通过位于本地目录下的子目录'functions'和'pure_functions'中的YAML配置文件定义和加载的。

**function_cfg_dir (str)属性**: 存储函数配置文件所在的目录路径。

**pure_function_cfg_dir (str)属性**: 存储纯函数配置文件所在的目录路径。

**function_cfgs (dict)属性**: 用于存储所有加载的函数配置。

**__init__方法**: 使用给定的函数配置文件目录初始化FunctionManager类。

参数:
- function_cfg_dir (str): 函数配置文件所在的目录路径。
- pure_function_cfg_dir (str): 纯函数配置文件所在的目录路径。

**get_function_schema方法**: 根据函数名称获取函数的模式。

参数:
- function_name (str): 函数的名称。

返回值:
- dict: 如果找到函数，则返回函数的模式。
- None: 如果找不到函数，则返回None。

**register_function方法**: 注册一个新的函数及其模式。

参数:
- function_schema (dict): 要注册的函数的模式。

**execute方法**: 根据函数名称执行一个函数。

参数:
- function_name (str): 要执行的函数的名称。
- return_generation_usage (bool, 可选): 如果设置为True，则还返回函数执行的用法。
- function_cfg (dict, 可选): 函数的配置。如果未提供，则从加载的函数中检索。
- **kwargs: 要执行的函数的参数。

返回值:
- Tuple[dict, Optional[dict]]: 包含执行结果和可选的函数执行用法的元组。

抛出异常:
- KeyError: 如果找不到函数配置。

**execute_ai_function方法**: 根据标签执行一个函数。

参数:
- ai_function_label (AIFunctionLabel): 要执行的函数的标签。
- **kwargs: 要执行的函数的参数。

返回值:
- Any: 执行函数的返回值。

**__getitem__方法**: 允许FunctionManager实例像字典一样使用，通过键（实际上是函数名称）调用execute方法。

参数:
- function_name (str): 要执行的函数的名称。
- return_generation_usage (bool, 可选): 如果设置为True，则还返回函数执行的用法。
- **kwargs: 要执行的函数的参数。

返回值:
- execute方法的返回值。

**__call__方法**: 允许FunctionManager实例可调用，直接调用execute方法。

参数:
- function_name (str): 要执行的函数的名称。
- return_generation_usage (bool, 可选): 如果设置为True，则还返回函数执行的用法。
- **kwargs: 要执行的函数的参数。

返回值:
- execute方法的返回值。

**注意**: 使用FunctionManager类时需要注意以下几点:
- 在初始化FunctionManager类时，需要提供函数配置文件的目录路径。
- 可以通过get_function_schema方法获取函数的模式。
- 可以通过register_function方法注册新的函数。
- 可以通过execute方法执行函数，并获取执行结果和用法。
- 可以通过execute_ai_function方法执行带有标签的函数。
- 可以通过getitem和call方法以字典或可调用对象的方式调用execute方法。

**输出示例**:
```python
function_manager = FunctionManager()
function_schema = function_manager.get_function_schema('simple_thought')
print(function_schema)
# Output: {'name': 'simple_thought', 'parameters': {'prompt': {'type': 'str', 'default': 'Enter your thought:'}}}

function_manager.register_function({'name': 'simple_thought', 'parameters': {'prompt': {'type': 'str', 'default': 'Enter your thought:'}}})
print(function_manager.function_cfgs)
# Output: {'simple_thought': {'name': 'simple_thought', 'parameters': {'prompt': {'type': 'str', 'default': 'Enter your thought:'}}}}

result = await function_manager.execute('simple_thought', prompt='Hello')
print(result)
# Output: {'prompt': 'Hello'}

result = await function_manager['simple_thought'](prompt='Hello')
print(result)
# Output: {'prompt': 'Hello'}
```
## FunctionDef __init__
**__init__函数**: 此函数的功能是初始化FunctionManager类，并加载指定目录下的函数配置文件。

此函数是FunctionManager类的构造函数，它接受两个参数，分别指定函数配置文件和纯函数配置文件的目录路径。构造函数会初始化两个成员变量，分别用于存储这些配置文件的路径和配置信息。

具体来说，构造函数首先将传入的`function_cfg_dir`和`pure_function_cfg_dir`参数分别赋值给类的成员变量`self.function_cfg_dir`和`self.pure_function_cfg_dir`。这两个成员变量用于存储函数配置文件和纯函数配置文件的目录路径。

接下来，构造函数创建一个空字典`self.function_cfgs`，用于存储从配置文件中加载的函数配置信息。字典的键是函数的名称，值是对应的配置信息。

然后，构造函数使用`glob`模块搜索`self.function_cfg_dir`目录下的所有`.yaml`和`.yml`文件。对于每个找到的配置文件，它使用`open`函数以只读模式打开文件，并使用`yaml.load`函数加载文件内容，将其解析为Python字典。解析后的配置信息被添加到`self.function_cfgs`字典中，其中函数的名称作为键。

同样的过程也适用于`self.pure_function_cfg_dir`目录下的配置文件，不同之处在于纯函数配置文件可能包含多个函数的配置信息。因此，构造函数会遍历配置文件中的每个函数配置，并将它们添加到`self.function_cfgs`字典中。

**注意**:
- 在使用此构造函数时，需要确保传入的目录路径中确实存在`.yaml`或`.yml`的配置文件，否则`self.function_cfgs`将不会包含任何函数配置信息。
- 使用`yaml.load`函数时，指定了`Loader=yaml.FullLoader`参数，这是为了确保YAML文件被完全加载，包括所有复杂的YAML结构。
- 在处理文件和目录路径时，构造函数使用了`os.path`模块来确保路径的正确性，这使得代码更加健壮，能够在不同的操作系统上正确运行。
- 在实际部署和使用FunctionManager类时，需要注意配置文件的格式和内容，确保它们符合预期的结构，以便正确加载函数配置。
## FunctionDef get_function_schema
**get_function_schema函数**：该函数的作用是通过函数名称获取函数的模式。

该函数接受一个字符串类型的参数function_name，表示函数的名称。函数会根据给定的名称在函数配置中查找对应的函数模式。如果找到了对应的函数模式，则返回该函数的模式，如果未找到，则返回None。

该函数的使用示例：
```python
function_schema = function_manager.get_function_schema("function_name")
```

**注意**：在使用该函数时，需要确保传入的函数名称存在于函数配置中，否则会返回None。

**输出示例**：假设函数配置中存在名为"function_name"的函数模式，返回该函数的模式。如果函数配置中不存在该函数，则返回None。
## FunctionDef register_function
**register_function 函数**: 此函数的功能是注册一个新的函数及其模式。

此函数`register_function`用于将新的函数定义和其模式(schema)注册到系统中。它接收一个字典类型的参数`function_schema`，该字典包含了函数的名称和其他相关配置信息。

函数的详细分析如下：
- 首先，函数会检查`function_schema`字典中的`name`键是否已经存在于`self.function_cfgs`字典中。`self.function_cfgs`是一个存储已注册函数配置的字典。
- 如果`name`已存在，并且传入的`function_schema`与已存储的模式不同，函数会记录一条警告日志，并返回`False`，表示注册失败。这是为了避免同名但不同配置的函数被重复注册。
- 如果`name`不存在或者模式相同，则将`function_schema`存储到`self.function_cfgs`字典中，并返回`True`，表示注册成功。

在项目中，`register_function`函数被调用于`XAgent/tools/toolserver.py`文件中的两个地方：
1. `get_available_tools`方法中，用于注册从工具服务器获取的可用工具的模式。它会遍历所有工具，对于不在黑名单中的工具，尝试注册它们的模式。如果注册成功，该工具会被添加到有效工具列表中。
2. `get_schema_for_tools`方法中，用于注册特定工具的模式。它会向工具服务器请求特定工具的模式，并尝试注册这些模式。

**注意**：
- 在使用`register_function`函数时，需要确保传入的`function_schema`字典包含`name`键，且其值为字符串类型。
- 如果尝试注册的函数名称已存在，并且模式不一致，函数将不会被注册，并且会记录一条警告日志。
- 此函数不处理`function_schema`的验证，调用者需要确保传入的模式是有效和正确的。

**输出示例**：
假设我们有以下的`function_schema`字典：

```python
function_schema = {
    "name": "example_function",
    "description": "这是一个示例函数",
    "parameters": [
        {"name": "param1", "type": "string"},
        {"name": "param2", "type": "int"}
    ]
}
```

如果这是第一次注册`example_function`，或者其模式与已注册的模式相同，`register_function`函数将返回`True`。如果已存在不同模式的同名函数，则返回`False`。
## AsyncFunctionDef execute
**execute函数**：该函数用于执行指定名称的函数。

该函数接受以下参数：
- function_name（str）：要执行的函数的名称。
- return_generation_usage（bool，可选）：如果设置为True，则还返回函数执行的用法。
- function_cfg（dict，可选）：函数的配置。如果未提供，则从加载的函数中检索。
- **kwargs：要执行的函数的参数。

该函数返回一个元组，包含执行函数的返回值和可选的函数执行用法。

如果找不到函数配置，则会引发KeyError异常。

在函数内部，首先检查是否提供了函数配置。如果未提供函数配置且函数名称在self.function_cfgs中，则从self.function_cfgs中获取函数配置。否则，引发KeyError异常，提示找不到函数配置。

接下来，记录执行AI函数的日志。

然后，根据函数配置中的completions_kwargs参数，检查是否配置了模型。如果配置了模型，则尝试从CONFIG中获取该模型的配置。如果获取失败，则记录日志，并将completions_kwargs设置为空字典。

接着，根据函数配置中的function_prompt参数生成用户输入的消息。将用户输入的消息封装成列表形式。

根据CONFIG中的request.lib属性的值，分别处理不同的情况。如果request.lib为'openai'，则调用objgenerator.chatcompletion函数，传入相应的参数。如果request.lib为'xagent'，则调用objgenerator.chatcompletion函数，传入相应的参数。

最后，根据return_generation_usage参数的值，返回执行函数的返回值和函数执行用法（如果有）。

**注意**：在使用该函数时，需要确保已正确配置函数，并提供正确的参数。

**输出示例**：模拟代码返回值的可能外观。

```python
{
    "returns": {
        "arg1": "value1",
        "arg2": "value2"
    },
    "usage": {
        "duration": 2.5,
        "cost": 0.5
    }
}
```
## AsyncFunctionDef execute_ai_function
**execute_ai_function 函数**: 此函数的功能是执行指定标签的 AI 函数。

此函数是异步的，意味着它是在支持异步操作的环境中运行的，例如 Python 的异步 I/O 框架。它通过一个 AI 函数的标签来执行相应的函数，并接受任意数量的位置参数和关键字参数。

参数解析：
- `ai_function_label` (AIFunctionLabel): 需要执行的 AI 函数的标签。这个标签包含了函数的名称、描述、参数模式(schema)和其他执行所需的关键字参数。
- `*args`: 位置参数列表，用于传递给 AI 函数。
- `**kwargs`: 关键字参数字典，用于传递给 AI 函数。特别地，`kwargs` 可以包含一个 "messages" 键，其值是一个消息列表，这些消息将被用于 AI 函数的执行上下文中。

函数执行流程：
1. 首先，函数会记录正在执行的 AI 函数的标签名称。
2. 然后，它会处理 `kwargs` 中的 "messages"，并构建一个新的消息列表，这个列表包含了用户角色的消息，内容是根据 AI 函数的提示(prompt)和提供的参数格式化后的文本。
3. 接下来，函数会调用 `objgenerator.chatcompletion` 方法，该方法会根据提供的消息和工具信息来生成回复和工具调用结果。
4. 最后，函数会根据 AI 函数的返回类型标签对工具调用的结果进行验证，并返回验证后的结果。

注意事项：
- 在调用此函数时，需要确保 `ai_function_label` 是有效的，并且提供的参数与 AI 函数的预期参数相匹配。
- 由于此函数是异步的，调用它时需要使用 `await` 关键字，并且调用者也应该在异步环境中。

在项目中的调用情况：
在 `XAgent/ai_functions/wrapper.py` 文件中，`execute_ai_function` 函数被封装在一个名为 `wrapper` 的异步函数中。`wrapper` 函数会将关键字参数转换为字符串格式，并调用 `execute_ai_function` 函数，传递 AI 函数的标签和其他参数。

**输出示例**:
假设 AI 函数的返回类型是一个字符串，那么 `execute_ai_function` 函数可能返回如下值：
```python
"AI 函数执行的结果"
```

这个返回值是 AI 函数执行后的结果，其类型和内容取决于具体的 AI 函数。
## AsyncFunctionDef __getitem__
**__getitem__ 函数**: 此函数的功能是允许 FunctionManager 实例像字典一样行为，通过键（实际上是函数名称）调用 execute 方法。

此函数是 FunctionManager 类的异步魔术方法，它允许开发者使用字典的索引访问方式来执行存储在 FunctionManager 中的函数。当你尝试通过函数名来获取某个函数时，这个方法会被调用，并且会执行相应的函数。

具体来说，这个方法接受以下参数：
- `function_name` (str): 要执行的函数的名称。
- `return_generation_usage` (bool, optional): 可选参数，默认为 False。如果设置为 True，除了函数执行结果外，还会返回函数执行的使用情况。
- `**kwargs`: 要执行的函数的参数。

函数的执行流程如下：
1. 当你通过 `FunctionManager实例[函数名]` 的方式访问时，`__getitem__` 方法会被触发。
2. 方法内部会调用 `self.execute` 方法，将 `function_name`、`return_generation_usage` 和任何额外的关键字参数 (`**kwargs`) 传递给它。
3. `self.execute` 方法会处理函数的执行，并返回结果。
4. 如果 `return_generation_usage` 被设置为 True，那么除了返回函数的执行结果外，还会返回函数执行的使用情况。

**注意**：
- 由于这是一个异步方法，因此在调用时需要使用 `await` 关键字。
- 在使用此方法时，需要确保 FunctionManager 中已经注册了 `function_name` 对应的函数。
- 传递给函数的参数应该与该函数定义时期望的参数匹配。

**输出示例**：
假设 FunctionManager 实例中注册了一个名为 "add" 的函数，该函数接受两个参数 `x` 和 `y` 并返回它们的和。那么调用 `__getitem__` 方法的返回值可能如下：

```python
# 假设 function_manager 是 FunctionManager 的一个实例
result = await function_manager['add'](x=10, y=20)
print(result)  # 输出: 30
```

如果 `return_generation_usage` 设置为 True，返回值可能包含函数执行的使用情况，例如：

```python
result, usage = await function_manager['add'](x=10, y=20, return_generation_usage=True)
print(result)  # 输出: 30
print(usage)   # 输出: 函数执行的使用情况（具体内容取决于 execute 方法的实现）
```
## AsyncFunctionDef __call__
**__call__函数**: 该函数的功能是允许FunctionManager实例像函数一样被调用，直接调用execute方法。

该`__call__`方法是一个异步方法，它允许FunctionManager的实例像普通函数一样被调用。当你创建了FunctionManager的实例后，你可以直接使用实例名加上括号的方式来执行一个函数，而不需要显式地调用execute方法。这样做的好处是可以让代码更加简洁，使用起来更加直观。

具体来说，当你调用这个`__call__`方法时，你需要传递一个函数名（`function_name`），这是你想要执行的函数的名称。此外，你还可以选择性地传递一个布尔值（`return_generation_usage`），默认为False，当设置为True时，除了函数执行的结果外，还会返回函数执行的使用情况。最后，你可以传递任意数量的关键字参数（`**kwargs`），这些参数将被用作执行函数时的参数。

在函数内部，`__call__`方法将会异步调用`execute`方法，传递给它所有接收到的参数，并返回`execute`方法的执行结果。由于这是一个异步方法，因此在调用时需要使用`await`关键字。

**注意**：
- 在使用这个`__call__`方法时，需要确保你的环境支持异步调用。
- 由于`__call__`方法是异步的，因此你需要在一个异步环境中调用它，比如在一个异步函数中或者使用异步事件循环。
- 传递给`__call__`方法的关键字参数应该与你想要执行的函数所期望的参数相匹配。

**输出示例**：
假设`execute`方法执行后返回了一个字符串"Hello, World!"，那么调用`__call__`方法的可能输出如下：
```python
# 假设function_manager是FunctionManager的一个实例
result = await function_manager('greet', name='World')
print(result)  # 输出: Hello, World!
```
如果`return_generation_usage`被设置为True，并且`execute`方法返回了一个包含结果和使用情况的元组，那么调用`__call__`方法的可能输出如下：
```python
result, usage = await function_manager('greet', return_generation_usage=True, name='World')
print(result)  # 输出: Hello, World!
print(usage)   # 输出: 使用情况的一些信息
```
***
