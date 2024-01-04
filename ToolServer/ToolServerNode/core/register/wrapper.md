# FunctionDef generate_tool_labels
**generate_tool_labels函数**: 该函数的作用是为提供的函数生成并返回工具标签。如果工具未启用，则会打印调试日志信息并返回None。

参数:
- name (str, 可选): 工具的名称。如果未指定，则使用函数的名称。
- enabled (bool, 可选): 确定工具是否启用。默认为True。
- disabled_reason (Optional[str], 可选): 工具禁用的原因。默认为None。
- func (Callable[..., Any], 可选): 生成工具标签的函数。默认为None。
- visible(bool, 可选): 工具的可见性状态。默认为True。

返回值:
- Union[ToolLabels,None]: 包含工具信息的ToolLabels对象，如果工具未启用则返回None。

该函数首先检查工具是否启用，如果未启用，则根据禁用原因打印调试日志信息并返回None。

接下来，函数会检查方法是否具有完整的注释。它会解析函数的文档字符串，并根据解析结果生成自动签名。自动签名是一个字典，包含参数的类型、描述和默认值（如果有）。同时，它会将非可选参数添加到required列表中。

然后，函数会根据提供的参数生成工具的名称和描述。如果函数的文档字符串中包含简短描述和长描述，则将它们合并为描述。

最后，函数会返回一个ToolLabels对象，其中包含工具的名称、描述、方法、签名、必需参数、启用状态和禁用原因。

**注意**: 使用该代码的注意事项是...

**输出示例**:
```
ToolLabels(
    name='generate_tool_labels',
    description='该函数的作用是为提供的函数生成并返回工具标签。如果工具未启用，则会打印调试日志信息并返回None。',
    method=<function generate_tool_labels at 0x00000123456789>,
    signature={
        'name': {'type': 'str', 'description': '工具的名称'},
        'enabled': {'type': 'bool', 'description': '确定工具是否启用', 'default': True},
        'disabled_reason': {'type': 'Optional[str]', 'description': '工具禁用的原因', 'default': None},
        'func': {'type': 'Callable[..., Any]', 'description': '生成工具标签的函数', 'default': None},
        'visible': {'type': 'bool', 'description': '工具的可见性状态', 'default': True}
    },
    required=['name'],
    enabled=True,
    disabled_reason=None,
    visible=True
)
```
***
# FunctionDef toolwrapper
**toolwrapper函数**: 该函数的作用是将普通类或函数装饰为工具对象。

`toolwrapper` 函数是一个装饰器，它用于将普通的类或函数装饰成工具对象，使其可以在工具服务器中注册和使用。这个装饰器接受几个参数，包括：

- `name`: 工具的名称，默认为被装饰类或函数的名称。
- `enabled`: 表示工具是否启用，默认为True。
- `disabled_reason`: 如果工具未启用，这里可以提供未启用的原因，默认为None。
- `parent_tools_visible`: 表示父工具是否可见，默认取自配置文件中的`toolregister`部分的`parent_tools_visible`设置。
- `visible`: 表示工具是否可见，默认为True。

装饰器内部定义了一个`decorator`函数，它会检查传入的对象`obj`是类还是函数，并对其进行相应的处理：

1. 如果`obj`是一个类，装饰器会检查这个类是否是`BaseEnv`的子类。如果不是，会抛出异常。然后，它会为类中定义的每个函数生成工具标签（`tool_labels`），并将这些标签存储在类的`env_labels`属性中。

2. 如果`obj`是一个函数，装饰器会直接为这个函数生成工具标签，并将其存储在函数的`tool_labels`属性中。

在处理类时，装饰器还会处理类的继承关系，如果类不是直接继承自`BaseEnv`，它会检查父类中的工具，并根据`parent_tools_visible`参数决定是否在描述中提及父类工具的可见性。

**注意**:
- 使用`toolwrapper`装饰器时，需要确保被装饰的类是`BaseEnv`的子类。
- 被装饰的类或函数的`__doc__`字符串将被用作工具的描述。
- 如果`visible`参数设置为False，工具将不会在工具列表中显示，但仍然可以被调用。
- 如果`enabled`参数设置为False，需要提供`disabled_reason`来解释为什么工具被禁用。

**输出示例**:
假设有一个名为`MyTool`的类，使用`toolwrapper`装饰后，它的`env_labels`属性可能会是这样的：

```python
EnvLabels(
    name='MyTool',
    description='这是一个示例工具类。',
    subtools_labels={...},  # 存储子工具的标签
    defined_tools=['tool1', 'tool2'],  # 定义的工具函数名称列表
    cls=<class '__main__.MyTool'>,
    enabled=True,
    disabled_reason=None,
    visible=True
)
```

如果是一个函数`my_function`，使用`toolwrapper`装饰后，它的`tool_labels`属性可能会是这样的：

```python
ToolLabels(
    name='my_function',
    description='这是一个示例工具函数。',
    func=<function my_function at 0x10b9a1f28>,
    enabled=True,
    disabled_reason=None,
    visible=True,
    func_type='staticmethod'  # 函数类型，可能是'classmethod', 'instancemethod'或'staticmethod'
)
```
## FunctionDef decorator
**decorator函数**: 此函数的功能是将一个类或函数包装为工具环境（Tool Environment）或工具（Tool），并为其添加元数据标签。

decorator函数是一个高阶函数，它接收一个对象（可以是类或函数）作为参数，并返回一个经过包装的对象。这个函数主要用于在ToolServerNode的注册系统中，将普通的类或函数转换为工具环境或工具，并为其添加一些元数据，以便在工具服务器中进行管理和使用。

当传入的对象是一个类时，decorator函数会检查这个类是否是BaseEnv的子类。如果不是，会抛出异常。这是因为在这个系统中，所有的工具环境都应该继承自BaseEnv类。接着，函数会读取类的文档字符串（如果有的话）作为描述，并根据传入的参数决定是否将工具环境中的工具设置为可见。

接下来，函数会检查这个类是否直接继承自BaseEnv。如果不是，它会获取所有直接父类的名称，并根据parent_tools_visible参数决定是否在描述中添加关于父类工具的继承和可见性的信息。然后，函数会遍历所有父类，如果父类有env_labels属性，并且该属性是EnvLabels的实例，就会将父类的工具标签合并到当前类的工具标签中。

之后，函数会获取类中定义的所有函数名称，并为每个函数生成工具标签。这些标签包括函数的名称、是否启用、禁用原因、函数本身以及是否可见。函数还会检查每个函数是类方法、静态方法还是实例方法，并相应地设置工具标签的func_type属性。最后，所有生成的工具标签会被添加到类的env_labels属性中。

当传入的对象是一个函数时，decorator函数会为这个函数生成工具标签，并将其赋值给函数的tool_labels属性。

如果传入的对象既不是类也不是函数，函数会抛出NotImplementedError异常。

**注意**:
- 在使用decorator函数时，需要确保传入的类是BaseEnv的子类，否则会抛出异常。
- 传入的类或函数需要有适当的文档字符串，以便生成描述信息。
- 如果需要隐藏父类的工具，可以设置parent_tools_visible参数为False。
- 如果需要隐藏当前工具环境或工具，可以设置visible参数为False。

**输出示例**:
```python
@decorator
class MyEnv(BaseEnv):
    """这是一个示例工具环境。"""

    def tool1(self):
        """这是一个示例工具。"""
        pass

# 经过decorator函数装饰后，MyEnv类将具有env_labels属性，其中包含了工具环境的元数据标签。
```

在这个示例中，MyEnv类被装饰为一个工具环境，其内部定义的tool1函数被装饰为一个工具。通过env_labels属性可以访问到关于MyEnv及其工具的元数据信息。
***
