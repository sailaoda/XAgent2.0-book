# ClassDef ToolCall
**ToolCall函数**：ToolCall类的功能是表示一个工具调用的对象。

ToolCall类是一个用于表示工具调用的对象。它具有以下属性：
- id：表示工具调用的唯一标识符，采用UUID生成。
- type：表示工具的类型，使用ToolType枚举类型。
- name：表示工具的名称。
- arguments：表示工具调用的参数，可以是字符串或字典类型。
- status：表示工具调用的状态，使用ToolCallStatusCode枚举类型。
- output：表示工具调用的输出结果。
- output_multimedia：表示工具调用的多媒体输出结果，是一个可选的列表。

ToolCall类还具有以下方法：
- to_dict()：将ToolCall对象转换为字典形式。
- images()：获取工具调用的多媒体输出结果中的图片。

ToolCall类还重写了__setattr__方法，用于对output属性进行特殊操作。

ToolCall类还定义了unwrap_output()方法，用于处理工具调用的输出结果。

ToolCall类还定义了set_tool()方法，用于设置工具调用的名称和参数。

ToolCall类还定义了_wrap_arguments()方法，用于将参数包装成字符串形式。

ToolCall类还定义了to_str()方法，用于将ToolCall对象转换为字符串形式。

ToolCall类还重写了__str__方法，用于将ToolCall对象转换为字符串形式。

**注意**：使用ToolCall类时需要注意以下几点：
- output属性的设置会触发特殊操作。
- 当工具调用的名称包含"filesystem"时，需要注意black_list参数中的限制。

**输出示例**：
[Tool Call] Unknown()
[Status] PENDING
[Output]
## FunctionDef to_dict
**to_dict函数**: 该函数的功能是将ToolCall对象转换为字典格式。

该`to_dict`函数是`ToolCall`类的一个方法，它的作用是将`ToolCall`对象的属性转换成一个字典（dict）格式，以便于进行JSON序列化或者其他需要字典格式的场合。这个方法通常用于记录或者传输对象的状态。

具体来说，`to_dict`方法首先调用`self.model_dump(mode="json")`来获取对象的基本属性，并将其存储在变量`d`中。这个`model_dump`方法可能是一个将对象属性转换为JSON格式的自定义方法。

接下来，`to_dict`方法检查`self.output`属性。如果`self.output`不是一个空字符串，那么它会被添加到字典`d`中。这意味着如果`ToolCall`对象有输出内容，它将被记录下来。

然后，方法检查`self.output_multimedia`属性。如果这个属性不是`None`，即存在多媒体输出，它也会被添加到字典`d`中。这可能包括图像、视频等多媒体内容的链接或数据。

最后，`to_dict`方法返回构建好的字典`d`。

在项目中，`to_dict`方法被以下两个文件中的代码调用：

1. 在`XAgent/agent/base.py`文件中，`to_dict`方法被用于将`ToolCall`对象转换为字典格式，以便在记录代理步骤（agent step）时，可以将工具调用（tool calls）的信息以JSON格式写入日志或数据库。

2. 在`XAgent/record.py`文件中，`to_dict`方法同样被用于将`ToolCall`对象转换为字典格式，并将其序列化为JSON，然后写入文件系统或数据库。如果`ToolCall`对象包含多媒体输出，这些内容也会被相应地处理和存储。

**注意**：
- 在使用`to_dict`方法时，需要确保`ToolCall`对象的`model_dump`方法能够正确地将对象属性转换为字典格式。
- 如果`ToolCall`对象包含多媒体输出，需要额外处理这些输出，例如将图片数据从Base64编码转换为二进制数据。

**输出示例**：
```python
{
    "id": "123456",
    "name": "example_tool",
    "output": "这是工具的输出结果",
    "output_multimedia": [
        {
            "type": "image_url",
            "url": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..."
        }
    ]
}
```
以上示例展示了`to_dict`方法可能返回的字典格式。其中包含了工具调用的ID、名称、文本输出以及多媒体输出的列表。
## FunctionDef images
**images函数**：这个函数的作用是从self.output_multimedia中筛选出type为'image_url'的元素，并返回一个列表。

该函数首先判断self.output_multimedia是否为None，如果是，则返回None。然后使用filter函数筛选出type为'image_url'的元素，并将其存入ret列表中。如果ret列表为空，则返回None。最后返回ret列表作为函数的输出。

注意：使用该函数时需要确保self.output_multimedia的格式正确，并且包含了'type'字段。

输出示例：
```
[
    {
        'type': 'image_url',
        'url': 'https://example.com/image1.jpg'
    },
    {
        'type': 'image_url',
        'url': 'https://example.com/image2.jpg'
    }
]
```
## FunctionDef __setattr__
**__setattr__ 方法的功能**: 该方法的功能是在给对象的属性赋值时提供自定义行为。

当在一个类中定义了 `__setattr__` 方法时，每次尝试给对象的属性赋值时，都会调用这个方法。这个方法接收两个参数：`name` 和 `value`，其中 `name` 是属性的名称，`value` 是要赋给属性的值。

在这段代码中，`__setattr__` 方法被用来对特定属性 `output` 进行自定义处理。如果尝试设置的属性名称是 `output`，那么在实际赋值之前，会先调用 `self.unwrap_output(value)` 方法来处理这个值。这个处理过程可能涉及到数据的转换或者验证等操作。在这个例子中，注释中提到的“例如，转换为大写”只是一个示例，具体的 `unwrap_output` 方法实现细节并没有给出。

在对 `output` 属性进行特殊处理之后，代码使用 `super().__setattr__(name, value)` 调用基类的 `__setattr__` 方法，以确保除了 `output` 之外的其他属性可以正常地被设置。

**注意**:
- 当覆盖 `__setattr__` 方法时，需要确保对于不需要特殊处理的属性，依然能够正常赋值。这通常通过调用基类的 `__setattr__` 方法来实现。
- 如果在 `__setattr__` 方法中不慎引入了错误，可能会导致无法预料的行为，因为这会影响到所有属性的设置过程。
- 在使用 `__setattr__` 方法时，要特别注意避免无限递归的情况。例如，在 `__setattr__` 方法内部直接对属性赋值（如 `self.name = value`）会再次触发 `__setattr__`，从而形成递归调用。正确的做法是使用 `super().__setattr__(name, value)` 来避免这种情况。
- 如果 `unwrap_output` 方法依赖于对象的其他状态或属性，需要确保在调用 `unwrap_output` 之前这些状态或属性已经被正确设置。
## FunctionDef unwrap_output
**unwrap_output 函数**: 该函数的功能是解析工具调用的输出值，将其转换为适当的格式，并处理多媒体数据。

该函数接收一个参数 `value`，这个参数代表工具调用的输出值。函数的主要目的是检查输出值是否为多媒体数据，并根据数据类型进行相应的处理。

函数内部定义了一个名为 `check_output` 的嵌套函数，用于递归地检查和处理输出值。如果输出值是一个字典，并且包含 `type` 和 `data` 键，那么根据 `type` 的值（`simple`、`binary` 或 `composite`），函数会执行不同的操作：

- 当 `type` 为 `simple` 时，直接返回 `data` 中的数据。
- 当 `type` 为 `binary` 时，如果 `media_type` 表示的是图像类型（例如以 `image` 开头），则将图像数据转换为 Base64 编码的图片 URL，并添加到 `output_multimedia` 列表中。如果 `media_type` 是未知的，会打印警告信息，并将原始数据添加到 `output_multimedia` 列表中。
- 当 `type` 为 `composite` 时，表示输出值是由多个部分组成的复合数据。函数会遍历 `data` 列表中的每个元素，递归调用 `check_output` 函数处理，并将返回的非 `None` 结果收集到一个列表中返回。

如果输出值是基本数据类型（字符串、整数、浮点数、布尔值或列表），则直接返回该值。如果输出值为 `None`，则返回 `None`。如果输出值是未知类型，则抛出 `TypeError` 异常。

在 `ToolCall` 类的 `__setattr__` 方法中，当设置 `output` 属性时，会调用 `unwrap_output` 函数来处理输出值。这样，每次工具调用的输出被设置时，都会自动进行解析和格式化。

**注意**：
- 在处理多媒体数据时，需要注意 `media_type` 的类型，目前函数只处理了以 `image` 开头的媒体类型。
- 如果输出数据类型未知，函数会抛出 `TypeError` 异常，调用者需要对异常进行处理。

**输出示例**：
假设工具调用返回了以下输出值：
```python
{
    "type": "composite",
    "data": [
        {"type": "simple", "data": "文本信息"},
        {"type": "binary", "media_type": "image/jpeg", "data": "base64编码的图片数据"}
    ]
}
```
调用 `unwrap_output` 函数后，会返回以下结果：
```python
[
    "文本信息",
    None  # 图片数据被转换为图片 URL 并存储在 `output_multimedia` 属性中
]
```
同时，`output_multimedia` 属性将包含一个包含图片 URL 的字典：
```python
[
    {
        "type": "image_url",
        "image_url": {
            "url": "data:image/jpeg;base64,base64编码的图片数据"
        }
    }
]
```
### FunctionDef check_output
**check_output函数**: 此函数的功能是检查并处理工具调用的输出结果。

check_output函数接收一个参数obj，该参数是一个可能包含工具调用输出的对象。函数的主要目的是根据输出对象的类型和内容，提取或转换为适当的格式，以便进一步处理或显示。

函数首先检查obj是否是字典类型。如果是，它会进一步检查字典中是否包含键'type'和'data'，并且'type'的值属于['simple', 'binary', 'composite']之一。根据'type'的值，函数会采取不同的处理方式：

- 如果'type'的值是'simple'，函数直接返回obj中'data'键对应的值。
- 如果'type'的值是'binary'，函数会检查'media_type'是否以'image'开头。如果是，它会将图像数据作为base64编码的URL添加到self.output_multimedia列表中。如果'media_type'不是图像类型，函数会打印一条警告信息，并将媒体类型和数据添加到self.output_multimedia列表中。在这种情况下，函数返回None。
- 如果'type'的值是'composite'，函数会遍历'data'键对应的列表，并递归调用check_output函数处理每个元素。只有当递归调用的结果不是None时，才会将结果添加到返回列表中。

如果obj不是字典类型，函数会检查它是否是字符串、整数、浮点数、布尔值或列表类型。如果是这些类型之一，函数直接返回obj。

如果obj是None，函数返回None。

如果obj不属于上述任何一种类型，函数会抛出TypeError异常。

**注意**：
- 在使用check_output函数时，需要确保传入的obj参数格式正确，否则可能会抛出异常。
- 此函数设计为内部使用，通常不应直接从外部调用。
- 函数中使用了Python 3.10引入的模式匹配（match-case）语法，因此需要Python 3.10或更高版本才能运行。

**输出示例**：
假设我们有以下工具调用的输出结果：

```python
output_simple = {'type': 'simple', 'data': 'Hello, World!'}
output_binary_image = {'type': 'binary', 'media_type': 'image/jpeg', 'data': 'base64encodedstring'}
output_composite = {'type': 'composite', 'data': [output_simple, output_binary_image]}
```

调用check_output函数处理这些输出将得到以下结果：

```python
# 对于简单类型的输出
result_simple = check_output(output_simple)  # 返回 'Hello, World!'

# 对于二进制图像类型的输出
result_binary_image = check_output(output_binary_image)  # 返回 None，并且self.output_multimedia被更新

# 对于复合类型的输出
result_composite = check_output(output_composite)  # 返回 ['Hello, World!']
```

在处理二进制图像类型的输出时，self.output_multimedia可能会被更新为以下形式：

```python
self.output_multimedia = [
    {
        "type": "image_url",
        "image_url": {
            "url": "data:image/jpeg;base64,base64encodedstring"
        }
    }
]
```
## FunctionDef set_tool
**set_tool函数**: 该函数的功能是设置工具调用的名称和参数。

该`set_tool`函数是`ToolCall`类的一个方法，它用于设置工具调用的名称和参数。这个方法接受两个参数：`tool_name`和`tool_args`。`tool_name`是一个字符串，表示要调用的工具的名称；`tool_args`是一个字典，包含了调用工具时所需的参数。

在`set_tool`方法内部，它将`tool_name`赋值给对象的`name`属性，将`tool_args`赋值给对象的`arguments`属性。这样，`ToolCall`对象就被赋予了执行特定工具调用所需的所有信息。

在项目中，`set_tool`函数被`XAgent/agent/utils.py`文件中的`message_to_toolcall`函数调用。`message_to_toolcall`函数负责将消息转换为`ToolCall`对象。在这个过程中，如果消息中包含`tool_calls`键，它会使用消息中的第一个工具调用的名称和参数，调用`set_tool`方法来设置`ToolCall`对象。

例如，如果消息结构如下：
```python
{
    "content": "The content is useless",
    "tool_calls": [
        {
            "function": {
                "name": "xxx",
                "arguments": {
                    "arg1": "value1",
                    "arg2": "value2"
                }
            }
        }
    ],
    "arguments": {
        "arg1": "value1",
        "arg2": "value2"
    },
}
```
`message_to_toolcall`函数会从消息中提取工具调用的名称和参数，并使用`set_tool`方法将它们设置到`ToolCall`对象中。

**注意**:
- 在使用`set_tool`方法时，确保传入的`tool_name`和`tool_args`是有效的，因为这些值将直接影响工具调用的执行。
- 如果消息格式不正确或缺少必要的信息，应当在代码中进行适当的错误处理，以避免程序出现异常。在上述代码示例中，如果消息中没有`tool_calls`键，会记录一条警告日志。
## FunctionDef _wrap_arguments
**_wrap_arguments 函数**: 该函数的功能是将函数的参数以字符串的形式进行包装，并且可以排除黑名单中的关键词。

该函数 `_wrap_arguments` 主要用于将 ToolCall 对象中的 `arguments` 字典的键值对转换成一个字符串，以便于在日志或者其他输出中更加直观地展示这些参数。函数接受一个参数 `black_list`，这是一个字符串列表，用于指定在参数字符串中需要被排除的关键词。如果参数的键存在于黑名单中，该键的值将被替换为 '`wrapped`'。

函数内部首先定义了一个常量 `SINGLE_ACTION_MAX_LENGTH`，它来自配置文件中的 `execution.summary.single_step_max_tokens`，用于限制每个动作的最大长度。然后，函数初始化一个空字符串 `ret` 和一个参数长度计数器 `args_len`。

接下来，函数对 `self.arguments` 字典进行排序，排序依据是每个值转换为字符串后的长度。排序后，函数遍历排序的参数键值对。对于每个键值对，如果键存在于黑名单中，则值被替换为 '`wrapped`'。然后，使用 `clip_text` 函数来裁剪值的字符串表示，确保它不会使累积的参数字符串长度超过最大限制。如果裁剪后的长度加上已有的参数长度仍小于最大长度限制，则将键值对添加到返回字符串 `ret` 中。如果长度达到了限制，则在值的字符串表示后面添加省略号 '...'，并将其添加到 `ret`。

最后，函数返回 `ret` 字符串，但是去掉了最后一个逗号。

在项目中，`_wrap_arguments` 函数被 `ToolCall` 类的 `to_str` 方法调用。`to_str` 方法用于生成 ToolCall 对象的字符串表示，其中包括工具调用的名称、状态和输出。在生成工具调用的参数字符串时，`to_str` 方法会调用 `_wrap_arguments` 函数，并传入一个黑名单列表，以排除那些不应该显示的参数。

**注意**:
- 黑名单 `black_list` 应该只包含那些确实需要被隐藏的关键词。
- 返回的参数字符串长度受 `SINGLE_ACTION_MAX_LENGTH` 的限制，可能会对长参数进行裁剪。
- 在调用 `_wrap_arguments` 函数时，应确保 `self.arguments` 已经被正确地初始化并包含了所有需要的参数。

**输出示例**:
假设 `self.arguments` 包含 `{ 'path': '/home/user', 'recursive': True }`，且 `black_list` 为空，`SINGLE_ACTION_MAX_LENGTH` 足够长，那么函数的返回值可能是：
```
path="/home/user",recursive=True
```
如果 `black_list` 包含 `'recursive'`，则返回值可能是：
```
path="/home/user",recursive=`wrapped`
```
## FunctionDef to_str
**to_str函数**: 这个函数的功能是将工具调用对象转换为字符串表示。

该函数接受两个参数：return_output和black_list。return_output是一个布尔值，用于指定是否返回工具调用对象的输出结果。black_list是一个字符串列表，用于指定不包含在字符串表示中的属性。

函数首先检查工具调用对象的名称是否包含"filesystem"，如果是，则将"content"和"new_content"添加到black_list中。

然后，函数构建一个包含工具调用对象的名称和状态的列表ret。如果return_output为True，则将工具调用对象的输出结果添加到列表ret中。

接下来，函数导入XAgent.utils.token_count模块中的clip_text函数，并使用clip_text函数将列表ret转换为字符串表示。clip_text函数将根据指定的最大标记数对字符串进行剪裁，并返回剪裁后的字符串和剩余的标记数。

最后，函数返回剪裁后的字符串表示。

**注意**: 在使用该函数时，可以通过设置return_output参数来控制是否返回工具调用对象的输出结果。还可以通过设置black_list参数来指定不包含在字符串表示中的属性。

**输出示例**:
```
[Tool Call] tool_name(argument1=value1, argument2=value2)
[Status] status_name
[Output] output_value
```
## FunctionDef __str__
**__str__方法**: 该方法的功能是将对象转换为其字符串表示形式。

该`__str__`方法是Python中的一个特殊方法，用于定义当对象被传递给`str()`函数或者使用`print()`函数打印对象时，应该如何显示对象的字符串表示。在这段代码中，`__str__`方法通过调用另一个名为`to_str()`的方法来实现这一功能。

具体来说，当你尝试打印该对象或者在需要字符串表示的场景下使用该对象时，Python解释器会自动调用`__str__`方法。例如，如果你有一个名为`tool_call`的该类的实例，当你执行`print(tool_call)`时，实际上是在调用`tool_call.__str__()`方法。

在这个特定的实现中，`__str__`方法没有直接编写返回字符串的逻辑，而是委托给了`to_str()`方法。这意味着`to_str()`方法负责生成对象的字符串表示，而`__str__`方法则是一个简单的接口，将其工作委托出去。

**注意**:
- 确保类中存在`to_str()`方法，并且该方法返回一个字符串，否则在调用`__str__`方法时会引发错误。
- `__str__`方法应该返回一个易于理解的字符串，以便于开发者或用户理解对象的状态或内容。

**输出示例**:
假设`to_str()`方法返回了对象的名称和一些状态信息，那么`__str__`方法可能会返回类似于以下的字符串：
```
"ToolCall: 状态=成功, 名称=数据分析工具"
```
这个字符串表示了一个名为"数据分析工具"的`ToolCall`对象，其状态为"成功"。
***
