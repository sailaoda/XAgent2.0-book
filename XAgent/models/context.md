# ClassDef BaseContext
**BaseContext函数**：这个类的功能是表示基本的Agent上下文模型。

该类用于表示Agent的上下文模型，它包含了一些与Agent的上下文相关的属性和方法。该类的主要功能是将上下文的属性转换为字符串，并提供一个方法来获取上下文属性的字符串表示。

该类有一个私有方法`_value_to_str`，用于将上下文属性转换为字符串。该方法遍历上下文的属性，将不以`_`开头的属性转换为字符串，并根据属性的类型进行相应的处理。如果属性的值为None或空列表或空字符串或空字典，则忽略该属性。对于列表类型的属性，将列表中的元素按照一定的格式进行拼接。对于BaseModel类型的属性，将其转换为字符串表示。最后，将属性的名称进行格式化处理，并将属性的名称和值拼接为一个字符串，并将其存储在一个字典中。

该类还重写了`__str__`方法，用于将上下文属性的字符串表示拼接为一个字符串，并返回该字符串。

**注意**：该类的属性值必须是可序列化的类型，否则在转换为字符串时可能会出现错误。

**输出示例**：
```
# 属性1
属性1的值

# 属性2
属性2的值

...
```
## FunctionDef __str__
**__str__ 方法**: 该方法的功能是将对象的状态以字符串形式返回。

该`__str__`方法是Python中的一个特殊方法，用于定义对象被`str()`函数调用或在使用`print()`函数打印对象时应该显示的字符串。在这个特定的上下文中，`__str__`方法被用于返回一个代表对象状态的字符串。

具体到这段代码，`__str__`方法通过调用`_value_to_str()`方法获取到一个字典，其中包含了对象的状态信息。然后，它使用`join()`函数，以换行符(`'\n'`)为分隔符，将这些状态信息连接成一个单一的字符串。这样，当打印或转换对象为字符串时，可以得到一个包含所有状态信息的多行字符串。

这里的`_value_to_str()`方法应该是该对象的一个内部方法，它返回一个字典，其中的值是字符串形式的对象状态信息。这个方法的具体实现在这段代码中没有给出，但我们可以假设它负责将对象的属性或成员变量转换为易于阅读的字符串形式。

**注意**：使用这段代码时，需要确保`_value_to_str()`方法返回的字典中的值都是字符串类型，否则`join()`函数可能会抛出异常。

**输出示例**：
假设`_value_to_str()`方法返回的字典如下：
```python
{
    "属性1": "值1",
    "属性2": "值2",
    "属性3": "值3"
}
```
那么`__str__`方法的输出将会是：
```
值1
值2
值3
```
每个值占据一行，整个输出是这些值的集合，由换行符分隔。
***
# ClassDef LongTermContext
**LongTermContext功能**: 此类的功能是存储和管理长期上下文信息，这些信息不会频繁更新。

LongTermContext类继承自BaseContext，是一个长期代理上下文模型，用于包含偶尔更新的上下文信息。它定义了以下属性：

- `user_message`: 一个字符串列表，用于存储用户消息。
- `past_tasks`: 一个任意类型的列表，用于记录过去的任务。
- `experience`: 一个字符串，用于描述代理的经验。
- `skills_description`: 一个字符串列表，用于描述代理的技能。
- `ideas`: 一个字符串列表，用于存储代理的想法。
- `current_goal`: 一个字符串，用于记录当前的目标。
- `available_tools`: 一个字典列表，用于存储可用的工具信息。
- `resource`: 一个字符串列表，预定义了代理可以访问的资源。

此类还包含一个私有方法`_value_to_str`，它将类的属性转换为字符串表示，特别是`available_tools`属性，它会生成一个包含工具名称和描述的字符串列表。

在项目中，LongTermContext类被用于以下场景：

1. 在`XAgent/agent/base.py`文件中，`LongTermContext`被用于初始化代理的长期上下文信息。在`Agent`类的构造函数中，创建了一个`LongTermContext`实例，并将其赋值给`self.longterm_context`属性。

2. 在`XAgent/agent/summarize.py`文件中，`LongTermContext`实例被用作`summarize_tool_calls`函数的参数之一。该函数负责汇总工具调用的信息，并返回工具调用描述、额外的想法和令牌数。

**注意**：
- 当使用`LongTermContext`类时，需要注意它是用于存储不频繁更新的上下文信息，因此在更新这些信息时应谨慎。
- 在调用`_value_to_str`方法时，需要确保`available_tools`属性中的工具字典格式正确，以便正确生成工具的描述信息。

**输出示例**：
假设`LongTermContext`实例的`available_tools`属性包含以下工具信息：

```python
[
    {'type': 'function', 'function': {'name': 'Search', 'description': 'Perform a search operation'}},
    {'type': 'utility', 'utility': {'name': 'File Explorer'}}
]
```

调用`_value_to_str`方法后，可能得到的返回值示例为：

```python
{
    'available_tools': '# Available Tools\n0. Search - Perform a search operation\n1. utility'
}
```

这个返回值包含了一个描述可用工具的字符串，其中每个工具都有一个索引、名称和描述（如果有）。
***
# ClassDef WorkingContext
**WorkingContext 功能**: 此类的功能是存储和管理工作中代理的上下文信息，包括基本信息、观察结果、思考内容、工具调用记录和总结。

**详细代码分析**:
`WorkingContext` 类继承自 `BaseContext`，用于表示工作中代理的上下文模型。这个模型包含了频繁更新的上下文信息。具体来说，它包含以下属性：

- `basic_info`: 字符串类型，用于存储基本信息。
- `observation`: 字符串类型，用于存储观察结果。
- `_thoughts`: 字典列表，用于存储代理的思考内容。
- `_tool_calls`: `ToolCall` 类型的列表，用于存储工具调用的记录。
- `_summary`: 字符串类型，用于存储总结信息。
- `_self_examination`: 字符串类型，用于存储自我检查信息。

类中定义了两个方法：

1. `_get_hidden`: 此方法用于获取隐藏的上下文信息，例如 `_summary` 和 `_self_examination`。如果 `_summary` 存在，则返回包含 `summary` 和 `self_examination` 的字典。如果 `_summary` 不存在，但 `_thoughts` 或 `_tool_calls` 存在，则返回包含 `thoughts` 和 `tool_calls` 的字典。

2. `_value_to_str`: 此方法用于将上下文信息转换为字符串表示形式。它首先调用基类的 `_value_to_str` 方法，然后更新返回的字典，加入由 `_get_hidden` 方法获取的隐藏信息。

在项目中，`WorkingContext` 类被 `XAgent/agent/base.py` 文件中的 `Agent` 类使用。`Agent` 类在初始化时会创建一个 `WorkingContext` 实例来存储工作上下文，并在需要时更新这个上下文。例如，`update_working_context` 方法会更新 `basic_info` 和 `observation` 属性。

**注意**:
- 在使用 `WorkingContext` 类时，需要注意其属性中有些是以单下划线 `_` 开头的，这表示它们是受保护的属性，不应该直接从类的外部访问或修改。
- `ToolCall` 类型的列表 `_tool_calls` 存储的是工具调用记录，需要确保 `ToolCall` 类已经被正确定义，并且可以被 `WorkingContext` 正确引用。

**输出示例**:
```python
{
    "summary": "# Summary\n这是一个总结示例。",
    "self_examination": "# Self Examination这是一个自我检查示例。",
    "thoughts": "# Thoughts\n[思考1] 这是一个思考内容的示例。",
    "tool_calls": "# Tool Calls\n1. 工具名称示例"
}
```
这个示例展示了如果 `WorkingContext` 类的 `_summary`、`_self_examination`、`_thoughts` 和 `_tool_calls` 属性都有值时，`_get_hidden` 方法可能返回的字典格式。
## FunctionDef _get_hidden
**_get_hidden函数**: 此函数的功能是获取隐藏的上下文信息并以字典形式返回。

_get_hidden函数是一个私有成员函数，它的作用是从Context对象中提取隐藏的信息，并将这些信息组织成一个字典返回。这些隐藏信息包括"summary"（摘要）、"self_examination"（自我检查）、"thoughts"（想法）和"tool_calls"（工具调用）。

函数首先创建一个空字典`ret`，用于存储提取的信息。然后，它检查`self._summary`属性是否包含内容。如果包含，它会将摘要信息添加到`ret`字典中，键为"summary"，值为一个字符串，包含标题和摘要内容。如果`self._self_examination`也包含内容，它会以类似的方式添加到`ret`字典中，键为"self_examination"。

如果`self._summary`为空，函数会检查`self._thoughts`和`self._tool_calls`属性。对于`self._thoughts`，它会将每个思考条目格式化为一个字符串，并将它们以"thoughts"为键添加到`ret`字典中。对于`self._tool_calls`，它会将每个工具调用的名称格式化为一个字符串，并以"tool_calls"为键添加到`ret`字典中。

最后，函数返回`ret`字典，其中包含了所有提取的隐藏信息。

**注意**：
- `_get_hidden`函数是一个私有函数，意味着它仅在定义它的类内部使用，不应该被类外部直接调用。
- 函数返回的字典中的键值对可能会根据实际的上下文信息有所不同，不一定每次都包含所有的键。

**调用情况**：
在`XAgent/models/context.py`文件中，`_get_hidden`函数被`_value_to_str`函数调用。`_value_to_str`函数首先调用其父类的同名函数获取基础的字符串表示，然后使用`_get_hidden`函数的返回值更新这个字符串表示。这表明`_get_hidden`函数用于在转换上下文信息为字符串表示时，添加额外的隐藏信息。

**输出示例**：
假设`self._summary`包含摘要信息，`self._thoughts`和`self._tool_calls`也有相应的内容，函数的返回值可能如下所示：
```python
{
    "summary": "# Summary\n摘要内容",
    "self_examination": "# Self Examination自我检查内容",
    "thoughts": "# Thoughts\n[想法1] 内容1\n[想法2] 内容2",
    "tool_calls": "# Tool Calls\n1. 工具名称1\n2. 工具名称2"
}
```
如果`self._summary`为空，而其他属性包含内容，返回的字典将不包含"summary"和"self_examination"键。
## FunctionDef _value_to_str
**_value_to_str函数**: 这个函数的作用是将对象的值转换为字符串。

该函数首先调用父类的_value_to_str函数，并将其返回值存储在ret变量中。然后，它调用self._get_hidden函数获取隐藏属性，并将其返回值更新到ret中。最后，函数返回ret。

在XAgent/agent/base.py文件中的get_history_message函数中调用了_value_to_str函数。该函数用于构建历史消息，包括过去的工作记忆。根据配置文件中的summary选项，函数会根据不同的情况构建不同的历史消息。

如果summary选项启用，函数会执行一系列异步任务，获取工具调用的摘要信息、推理过程以及上下文摘要信息。然后，根据工作上下文的历史记录构建历史消息。如果启用了多模态图像功能，函数还会将工具调用的图像添加到历史消息中。

如果summary选项未启用，函数会根据工作上下文的历史记录构建历史消息。对于每个工作上下文，函数会将工具调用的结果添加到历史消息中，并将助手上下文和基本信息添加到历史消息中。

需要注意的是，函数的返回值是一个包含历史消息的列表，每个历史消息是一个字典，包含角色和内容。

**注意**: 使用该代码时需要注意以下几点：
- 该函数依赖于父类的_value_to_str函数和self._get_hidden函数。
- 函数的返回值是一个包含历史消息的列表，每个历史消息是一个字典，包含角色和内容。

**输出示例**:
```
[
    {
        "role": "user",
        "content": "Your past thoughts & actions:\n..."
    },
    {
        "role": "assistant",
        "content": "# Basic Info\n...\n# Observation\n..."
    },
    {
        "role": "system",
        "content": "# Basic Info\n...\n# Observation\n..."
    },
    ...
]
```
***
