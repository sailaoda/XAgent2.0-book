# ClassDef OutputNotReady
**OutputNotReady类的功能**：该类是一个自定义异常，用于表示输出尚未准备好。

OutputNotReady类继承自Python标准库中的Exception类，是一个用于特定场景的自定义异常。当一个操作需要返回结果，但是结果尚未准备好时，可以抛出这个异常。这个类在构造函数中接收几个参数，包括可变参数`*args`，字符串类型的`type`，字符串类型的`next_calling`，以及字典类型的`arguments`。

- `*args`: 用于存储传递给异常的标准参数，这些参数通常用于描述异常的详细信息。
- `type`: 一个字符串，表示异常的类型，默认值为'retry'，这表明可能需要重试操作。
- `next_calling`: 一个字符串，表示下一次尝试调用的函数名，这可以帮助调用者知道下一步应该调用哪个函数来处理异常。
- `arguments`: 一个字典，包含下一次调用函数时需要使用的参数。

OutputNotReady类还定义了一个名为`next_try`的方法，该方法返回一个包含`type`、`next_calling`和`arguments`的字典，这有助于调用者了解如何处理异常。

在项目中，OutputNotReady类被用于以下场景：

1. 在`ToolServer/ToolServerNode/extensions/envs/shell.py`文件中，`read_stdout`和`write_stdin`方法会在输出尚未准备好时抛出OutputNotReady异常。这通常发生在异步或非阻塞的操作中，当立即需要结果，但结果尚未生成时。

2. 在`ToolServer/ToolServerNode/main.py`文件中，`execute_tool`函数会捕获OutputNotReady异常，并将其转换为HTTPException，状态码为450，异常的详细信息包含了如何进行下一次尝试的信息。

**注意**：
- 使用OutputNotReady异常时，应确保调用者能够处理这种异常，并根据异常提供的信息进行适当的重试或其他恢复操作。
- 在抛出OutputNotReady异常时，应提供足够的信息，以便调用者可以根据`next_calling`和`arguments`进行下一步操作。

**输出示例**：
如果`read_stdout`方法在没有可读输出时被调用，并且`probe`参数为True，那么可能会抛出如下异常：

```python
raise OutputNotReady('Output is not ready!', next_calling='read_stdout', arguments={'probe': True})
```

调用者可以根据异常中的信息来决定如何处理，例如，可以在一段时间后重试`read_stdout`方法，并将`probe`参数设置为True。
## FunctionDef next_try
**next_try函数**: 此函数的功能是准备下一次尝试，通过返回一个包含类型、下一次调用事件和参数的字典。

next_try函数是一个异常处理中使用的方法，它的目的是在异常发生时，提供一个重试的机制。当在ToolServerNode的主程序中执行工具时，如果遇到OutputNotReady异常，这个函数会被调用。它会返回一个字典，其中包含了异常处理所需的信息，如异常类型、下一次调用的事件以及调用时需要的参数。

具体来说，next_try函数返回的字典包含以下键值对：
- "type": 异常的类型，通常是异常类的名称。
- "next_calling": 下一次尝试调用的事件，可能是一个函数名或者一个特定的处理流程。
- "arguments": 下一次调用时需要的参数，这是一个字典，包含了执行工具所需的所有参数。

在ToolServerNode的main.py文件中，如果在执行工具的过程中捕获到OutputNotReady异常，会调用next_try函数，并将其返回的字典作为HTTPException的详细信息抛出。这样，前端或者调用者就可以根据这个详细信息来决定是否进行重试，以及如何进行重试。

**注意**：
- 使用next_try函数时，需要确保它是在异常处理的上下文中调用的，因为它依赖于异常实例的属性。
- 返回的字典应该包含足够的信息来指导调用者如何进行下一步操作，因此在设计异常类时，需要合理设置这些属性。

**输出示例**：
```python
{
    "type": "OutputNotReady",
    "next_calling": "retry_function",
    "arguments": {"param1": "value1", "param2": "value2"}
}
```
在这个示例中，返回的字典指示调用者异常类型为OutputNotReady，下一次尝试调用的事件是retry_function函数，且在调用时需要传递param1和param2两个参数。
***
# ClassDef ToolNotFound
**ToolNotFound函数**：这个类的功能是当找不到工具时引发自定义异常。

这个类继承自Exception类，用于在找不到工具时引发异常。它接受一个可变长度的参数列表和一个工具名称作为参数。工具名称是指找不到的工具的名称。

这个类有一个属性tool_name，用于存储工具的名称。

这个类还重写了__str__方法，用于返回带有工具名称的格式化异常错误消息。

在ToolServer/ToolServerNode/core/register/register.py文件中的__getitem__方法中，当找不到工具时，会引发ToolNotFound异常。在ToolServer/ToolServerNode/main.py文件中的execute_tool函数中，也会捕获ToolNotFound异常并返回相应的HTTP错误响应。

**注意**：在使用ToolNotFound类时，需要注意工具名称的正确性和存在性。

**输出示例**：假设工具名称为"tool1"，则返回的异常错误消息为"The tool tool1 is not found!"。
***
# ClassDef EnvNotFound
**EnvNotFound 功能**: 该类的功能是提供一个自定义异常，用于在环境变量未找到时抛出。

EnvNotFound 是一个自定义的异常类，它继承自 Python 的标准异常类 Exception。当在代码中需要访问某个环境变量，但是该变量不存在时，可以抛出这个异常来通知调用者存在问题。

这个类接受任意数量的参数（*args），这些参数将被存储在类的 `addition_info` 属性中。此外，它还接受一个名为 `env_name` 的参数，该参数指定了未找到的环境变量的名称，并将其存储在 `env_name` 属性中。

当 EnvNotFound 异常被转换为字符串表示（例如，通过 str 函数或在打印异常时），`__str__` 方法会生成一个格式化的错误消息，其中包含了未找到的环境变量的名称。

在项目中，EnvNotFound 被用于 ToolServerNode 的 register 模块中。当尝试获取一个环境配置字典，但是指定的环境名称 `env_name` 不在已注册的环境中时，或者指定的环境索引 `env_idx` 超出了范围时，将抛出 EnvNotFound 异常。这有助于调用者识别和处理环境配置相关的错误。

**注意**：
- 在使用 EnvNotFound 异常时，应确保提供了 `env_name` 参数，以便在异常消息中清楚地指出哪个环境变量未找到。
- 在捕获这个异常时，可以通过访问异常实例的 `env_name` 属性来获取未找到的环境变量的名称，以便进行进一步的错误处理或日志记录。

**输出示例**：
假设我们尝试获取名为 "DATABASE_URL" 的环境变量，但是该变量不存在，那么抛出的 EnvNotFound 异常可能会被转换为以下字符串：

```
The env DATABASE_URL is not found!
```

如果没有提供任何其他参数，只提供了 `env_name`，则输出可能如下：

```
The tool DATABASE_URL is not found!
```

这个字符串可以用于日志记录或显示给用户，以便他们了解发生了什么错误。
***
# ClassDef ToolRegisterError
**ToolRegisterError类的功能**：此类是一个自定义异常类，用于在注册工具时遇到错误时抛出。

ToolRegisterError类继承自Python的内置异常类Exception。当ToolServerNode的注册模块在尝试注册一个新工具时，如果遇到任何错误，例如代码执行失败、工具名称无法找到或者工具没有正确的标签，就会抛出这个自定义异常。

**详细代码分析**：
- `__init__` 方法接收任意数量的参数（*args），这些参数通常包含了错误信息，以及一个关键字参数`tool_name`，用于指定发生错误的工具的名称。
- `addition_info` 属性用于存储传递给异常的额外信息，通常是错误信息。
- `tool_name` 属性用于记录发生错误的工具名称。
- `__str__` 方法用于返回格式化的异常错误信息，其中包含了工具的名称。如果有额外的错误信息，它将被添加到错误消息中。

在ToolServerNode的注册模块中，`register_tool` 方法用于注册一个新的工具。如果在执行工具代码、评估工具名称或检查并注册工具函数时出现错误，将会捕获异常并记录错误信息，然后抛出ToolRegisterError异常。

**使用代码时的注意事项**：
- 当使用`register_tool` 方法注册工具时，如果遇到任何异常情况，应当捕获这些异常并适当处理，例如记录日志或者通知用户。
- ToolRegisterError异常的抛出应当包含足够的信息，以便于调试和错误追踪，这就要求在抛出异常时提供详细的错误信息和相关的工具名称。

**输出示例**：
如果在注册工具时代码执行失败，可能会看到如下的异常信息输出：

```
Failed to execute new tool code: [具体的错误信息]

Error happens when registering tool [工具名称]!
```

在这个示例中，"[具体的错误信息]"将会被替换为实际的错误描述，"[工具名称]"将会被替换为尝试注册的工具的名称。这样的输出有助于开发者快速定位问题所在。
## FunctionDef __str__
**__str__函数**: 该函数的功能是返回一个格式化的异常错误信息，其中包含了工具的名称。

这个`__str__`方法是一个特殊的方法，通常在Python中用于定义对象的字符串表示形式。当尝试将对象转换为字符串或在打印对象时，会自动调用这个方法。在这段代码中，`__str__`方法被用于定制异常信息的输出格式。

具体来说，这个方法首先调用了其超类（即基类或父类）的`__str__`方法，这通常是用来获取默认的异常信息。然后，它检查从超类获得的字符串`s`是否不为空。如果`s`不为空，说明超类已经提供了一些异常信息，那么它会在这个信息的基础上添加额外的内容，即在原有的异常信息后面追加一行，内容为“Error happens when registering tool {self.tool_name}!”，其中`{self.tool_name}`是一个占位符，它会被异常对象中的`tool_name`属性的值所替换。

如果从超类获得的字符串`s`为空，说明超类没有提供异常信息，那么它会直接创建一个新的字符串，内容同样是“Error happens when registering tool {self.tool_name}!”。

最后，这个方法返回最终的字符串`s`，这个字符串包含了完整的异常信息，包括工具名称。

**注意**：在使用这段代码时，需要确保异常类中有一个`tool_name`属性，否则在格式化字符串时会引发错误。此外，这个方法通常是在异常类中定义的，因此它通常与特定的异常类型相关联，用于提供关于异常的更多上下文信息。

**输出示例**:
如果`tool_name`属性的值为`"MyTool"`，那么这个方法可能返回如下字符串：
```
Exception has occurred: <异常类型>
Error happens when registering tool MyTool!
```
这里`<异常类型>`是超类`__str__`方法返回的异常信息。如果超类没有提供任何信息，那么输出将仅包含添加的工具注册错误信息。
***
# FunctionDef remove_color
**remove_color函数**: 该函数的功能是移除文本中的ANSI转义序列，即颜色代码。

`remove_color`函数接收一个字符串参数`text`，该参数包含可能带有ANSI颜色代码的文本。ANSI颜色代码通常用于在终端或命令行界面中设置文本颜色，它们是一系列以特殊字符序列（如`\x1b[31m`表示红色）开头的字符。这些序列在非终端环境中会显示为乱码，因此有时需要从文本中移除它们以获取纯净的文本内容。

函数内部使用正则表达式`ansi_escape`来匹配并替换掉所有的ANSI转义序列。这里的`ansi_escape`是一个预先定义的正则表达式对象，它能够识别出文本中所有的ANSI转义序列。函数通过调用`sub`方法，使用空字符串替换掉所有匹配到的转义序列，从而清除文本中的所有颜色代码。

在项目中，`remove_color`函数被调用于`ToolServer/ToolServerNode/core/exceptions.py`文件中。在这个文件中，`remove_color`函数用于在创建异常对象时，清理传入的错误信息字符串`error_msg`。如果`error_msg`是一个字符串，那么会先调用`remove_color`函数移除其中的颜色代码，然后再将清理后的错误信息传递给异常对象的构造函数。

**注意**：
- 在使用`remove_color`函数之前，需要确保`ansi_escape`正则表达式已经被正确定义和编译。
- 该函数假设输入的文本字符串`text`可能包含ANSI颜色代码，如果输入的文本中没有这些代码，函数将返回原始文本。
- 在处理异常信息或日志输出时，移除颜色代码可以避免在不支持颜色代码的环境中出现乱码。

**输出示例**：
假设输入的文本为`"\x1b[31m这是红色的文本\x1b[0m"`，调用`remove_color`函数后，返回的结果将是`"这是红色的文本"`。
***
# ClassDef ToolExecutionError
**ToolExecutionError函数**: 这个类的功能是在工具执行过程中遇到错误时引发自定义异常。

这个类继承自HTTPException类，它是一个自定义的异常类。在工具执行过程中遇到错误时，可以通过抛出ToolExecutionError异常来处理错误情况。

**参数**:
- error_msg (str): 工具执行过程中的错误消息。

**注意**: 在使用这个类时需要注意以下几点:
- error_msg参数必须是一个字符串类型。
- 如果error_msg参数包含颜色代码，需要使用remove_color函数将颜色代码移除。
- 在调用父类的构造函数时，传入的参数是HTTP状态码和错误消息。

这个类在项目中的以下文件中被调用:
- ToolServer/ToolServerNode/core/envs/pycoding.py
- ToolServer/ToolServerNode/core/envs/shell.py
- ToolServer/ToolServerNode/core/envs/web.py

在ToolServer/ToolServerNode/core/envs/pycoding.py文件中的_format_output函数中，当output的output_type为'error'时，会抛出ToolExecutionError异常。异常消息包含了cell_index和output的traceback信息。

在ToolServer/ToolServerNode/core/envs/shell.py文件中的execute函数中，当命令执行超时时，会抛出ToolExecutionError异常。异常消息包含了超时信息和命令执行的输出。

在ToolServer/ToolServerNode/core/envs/web.py文件中的_click_by_xpath函数中，如果找不到指定xpath的元素，则会抛出ToolExecutionError异常。异常消息包含了无效的xpath信息。

以上是对ToolExecutionError类的详细解释和代码分析。

**注意**: 在使用这个类时需要注意以下几点:
- 在捕获ToolExecutionError异常时，可以获取异常的错误消息进行处理。
- 在调用ToolExecutionError的构造函数时，需要传入错误消息作为参数。
## FunctionDef __init__
**__init__函数**: 该函数的功能是初始化一个异常对象，并设置错误消息和状态码。

该`__init__`函数是一个构造函数，用于创建一个新的异常对象。它接受一个参数`error_msg`，该参数是一个字符串，表示错误消息。

详细代码分析如下：
1. 函数首先检查传入的`error_msg`是否为字符串类型。这是因为错误消息应该是一个可读的文本信息。
2. 如果`error_msg`是字符串类型，那么会调用`remove_color`函数来去除可能存在的颜色编码。这通常用于清理从终端或日志文件中获取的文本，其中可能包含用于文本着色的特殊字符序列。
3. 在清理错误消息之后，通过`super().__init__`调用基类的构造函数。这里传递了两个参数：状态码`500`和清理后的错误消息`error_msg`。状态码`500`通常表示服务器内部错误。
4. 通过这种方式，异常对象被正确地初始化，包含了必要的错误信息和一个表示错误类型的状态码。

**注意**：
- 使用这段代码时，需要确保`remove_color`函数已经定义并且能够正确执行。否则，代码在执行时可能会引发另一个异常。
- 状态码`500`已经硬编码在构造函数中，这意味着所有使用这个异常类的错误都将被标记为服务器内部错误。如果需要表示不同类型的错误，可能需要修改这部分代码或者创建不同的异常类。
- 这个构造函数假设所有传入的错误消息都需要去除颜色编码，如果在某些情况下不需要这样处理，可能需要对构造函数进行相应的调整。
***
