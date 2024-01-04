# ClassDef BaseEnv
**BaseEnv函数**：BaseEnv类的功能是处理类和子类的函数和函数名称。该类提供了获取所有函数、定义函数及其名称的方法。它还确保在必要时进行配置更新。

**属性**：
- config（可选）：包含配置的字典。默认为空字典。

**__init__方法**：
初始化BaseEnv类，使用指定的或默认配置。

参数：
- config（可选）：包含配置的字典。默认为空字典。

注意：
- 为避免对原始对象进行修改，配置进行了深拷贝。

**__get_all_func_name__方法**：
获取类的所有函数名称，不包括以'_'字符开头的方法。

返回值：
- 包含函数名称的列表。

**__get_all_func__方法**：
获取类的所有函数，不包括以'__'字符开头的方法。

返回值：
- 包含函数的列表。

**__get_defined_func__方法**：
获取子类的所有函数，不包括以'_'字符开头的方法。

返回值：
- 包含子类定义函数的列表。

注意：
- 该方法从函数列表中删除了父类的方法，只提供在子类中新定义的函数。

**__get_defined_func_name__方法**：
获取子类的所有函数名称，不包括以'_'字符开头的方法。

返回值：
- 包含子类函数名称的列表。

该对象在以下文件中被调用：
- ToolServer/ToolServerNode/core/register/register.py
- ToolServer/ToolServerNode/core/register/wrapper.py
- ToolServer/ToolServerNode/extensions/envs/rapidapi.py
- ToolServer/ToolServerNode/extensions/envs/shell.py

在ToolServer/ToolServerNode/core/register/register.py文件中，该对象被以下代码调用：
```python
def get_func_name(func: Callable, env: BaseEnv = None) -> str:
    # ...
```
在ToolServer/ToolServerNode/core/register/wrapper.py文件中，该对象被以下代码调用：
```python
def decorator(obj:object)->Union[Type,Callable[..., Any]]:
    # ...
```
在ToolServer/ToolServerNode/extensions/envs/rapidapi.py文件中，该对象被以下代码调用：
```python
class RapidAPIEnv(BaseEnv):
    # ...
```
在ToolServer/ToolServerNode/extensions/envs/shell.py文件中，该对象被以下代码调用：
```python
class ShellEnv(BaseEnv):
    # ...
```

**注意**：在使用该对象时，需要注意配置的使用和相关的调用关系。

**示例输出**：
```python
# 示例输出
env = BaseEnv()
print(env.__get_defined_func_name__())  # ['func1', 'func2']
```
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化 BaseEnv 类，并使用指定或默认的配置。

该 `__init__` 函数是 BaseEnv 类的构造函数，其主要作用是初始化类实例。在这个函数中，首先会对一个全局的默认配置 `CONFIG` 进行深拷贝，以确保不会修改到原始的配置对象。随后，如果传入的 `config` 参数是一个字典，则会使用这个字典中的配置项更新当前实例的配置。此外，该函数还定义了一个 `env_labels` 属性，但在构造函数中并没有对其进行初始化。

具体到代码实现，函数接受一个名为 `config` 的参数，这个参数是一个字典，用于传递配置信息，默认为空字典。函数内部首先进行了一个深拷贝操作，复制了一个全局配置 `CONFIG`，然后检查传入的 `config` 是否为字典类型，如果是，则将其内容更新到实例的 `config` 属性中。最后，声明了一个 `env_labels` 属性，但未在构造函数中赋值。

在项目中，这个构造函数被多个环境类继承和调用，例如 `pycoding.py`、`shell.py`、`web.py` 和扩展环境 `rapidapi.py`、`shell.py`。这些子类通过调用 `super().__init__(config)` 来继承 BaseEnv 类的初始化过程，并在此基础上添加了各自特定环境的配置和初始化逻辑。例如，在 `pycoding.py` 中，除了调用基类的初始化函数，还设置了工作空间和笔记本配置，并确保工作空间的存在；在 `web.py` 中，除了基础配置，还进行了浏览器的设置任务。

**注意**：
- 在使用这个构造函数时，需要确保传入的 `config` 参数是一个字典，否则不会更新实例的配置。
- 由于 `env_labels` 在构造函数中没有初始化，因此在使用之前需要确保它被正确赋值，否则可能会引发错误。
- 在继承这个基类时，子类应该在调用 `super().__init__(config)` 后，根据自己的需要添加额外的初始化代码。
## FunctionDef __get_all_func_name__
**__get_all_func_name__ 函数**: 此函数的功能是获取类中所有公共方法的名称。

这个函数`__get_all_func_name__`定义在`ToolServer/ToolServerNode/core/base.py`文件中，它是一个类方法（由`cls`参数表示它可以直接通过类来调用，而不需要实例化）。此函数的目的是检索并返回类中所有的公共方法名称，即那些不以单个或双下划线`_`开头的方法。这通常用于反射（reflection）操作，比如动态地获取类的方法信息。

函数的实现使用了Python内置的`dir`函数和`getattr`函数。`dir(cls)`返回了一个包含`cls`类所有属性和方法名称的列表。列表推导式中的条件`not str(name).startswith('_')`用于过滤掉所有以单个下划线开头的私有或保护方法，`callable(getattr(cls, name))`则用于检查通过`getattr`获取的属性是否是可调用的，即是否是方法。最终，函数返回一个包含所有公共方法名称的列表。

在`ToolServer/ToolServerNode/core/base.py`文件中，`__get_all_func_name__`函数被`__get_all_func__`方法调用。`__get_all_func__`方法的目的是获取类中所有公共方法的引用，而不仅仅是方法的名称。它首先调用`__get_all_func_name__`来获取所有公共方法的名称，然后使用`map`函数和`getattr`函数结合这些名称来获取方法的引用。

**注意**：在使用`__get_all_func_name__`函数时，需要注意它只返回公共方法的名称，不包括私有方法（以单个或双下划线开头）和其他非方法属性。

**输出示例**：
假设有一个类`ExampleClass`，它有三个方法：`public_method`, `_protected_method`, `__private_method`。调用`ExampleClass.__get_all_func_name__()`将返回以下列表：
```python
['public_method']
```
这个列表中只包含了`public_method`，因为其他两个方法由于命名规则被视为非公共方法，因此不在返回列表中。
## FunctionDef __get_all_func__
**__get_all_func__ 函数**: 此函数的功能是获取类的所有函数，排除以 '__' 字符开头的方法。

此函数`__get_all_func__`定义在`ToolServer/ToolServerNode/core/base.py`文件中，它的作用是从一个类中提取出所有的公共函数（即那些不以双下划线开头的函数）。这个函数对于理解和使用类中定义的可调用方法非常有帮助，尤其是在需要动态获取这些方法进行操作时。

函数首先调用了`cls.__get_all_func_name__()`方法来获取类中所有函数的名称列表，这个方法应该返回一个包含函数名的列表，但具体实现并未在代码片段中给出。然后，它使用`map`函数结合`getattr`来获取类`cls`中每个函数名对应的函数对象。这里`[cls]*len(func_names)`创建了一个长度与`func_names`相同的列表，列表中的每个元素都是`cls`，这样做是为了在`map`函数中能够将每个函数名与类`cls`对应起来。

在项目中，`__get_all_func__`函数被`__get_defined_func__`方法调用。`__get_defined_func__`方法的目的是获取子类中定义的所有函数，排除那些从父类继承而来的函数。它首先调用`__get_all_func__`获取当前类的所有函数，然后遍历当前类的所有父类，对于每个父类，如果它不是`BaseEnv`的子类，则跳过；如果是，则调用父类的`__get_all_func__`方法获取父类的所有函数，并从当前类的函数列表中过滤掉这些父类的函数。最终返回的是一个只包含子类中新定义的函数的列表。

**注意**：
- 在使用`__get_all_func__`函数时，需要确保调用它的类中有`__get_all_func_name__`方法的实现，否则会抛出异常。
- 此函数返回的是函数对象的列表，而不是函数名的字符串列表。
- 由于此函数使用了双下划线前缀，它通常表示这是一个类的内部使用方法，不建议在类的外部直接调用。

**输出示例**：
假设有一个类`MyClass`，它有三个公共方法`func1`, `func2`, 和一个特殊方法`__func3__`。调用`MyClass.__get_all_func__()`可能会返回如下列表：
```python
[<function MyClass.func1>, <function MyClass.func2>]
```
注意，列表中不包含以双下划线开头的特殊方法`__func3__`。
## FunctionDef __get_defined_func__
**__get_defined_func__ 函数**: 此函数的功能是获取子类中定义的所有函数，排除以'_'字符开头的方法。

此函数`__get_defined_func__`是一个类方法，它用于获取一个类中定义的所有公共函数（即不以单下划线或双下划线开头的函数）。这个方法首先调用`__get_all_func__`方法来获取类中定义的所有函数，然后它会遍历该类的所有父类。如果父类不是`BaseEnv`的子类，则跳过该父类。对于是`BaseEnv`子类的父类，它会获取父类中定义的所有函数，并从当前类的函数列表中排除这些父类函数。这样做的目的是确保返回的函数列表只包含在当前子类中新定义的函数，而不包含从父类继承的函数。

在项目中，`__get_defined_func__`函数被`__get_defined_func_name__`函数调用。`__get_defined_func_name__`函数的目的是获取子类中定义的所有函数的名称，同样排除以'_'字符开头的方法。它通过调用`__get_defined_func__`来获取函数对象列表，然后使用`map`函数将每个函数对象的`__name__`属性（即函数的名称）提取出来，形成一个包含函数名称的列表。

**注意**：
- 使用此函数时，需要确保它是在一个类的上下文中调用的，且该类继承自`BaseEnv`或其子类。
- 此函数只返回那些在当前类中新定义的公共函数，不包括从父类继承的或以'_'开头的私有函数和特殊方法。

**输出示例**：
假设有一个名为`MyEnv`的类，它继承自`BaseEnv`，并且定义了两个函数`func1`和`func2`。同时，`BaseEnv`类定义了一个函数`base_func`。调用`MyEnv.__get_defined_func__()`将返回一个列表，其中包含`func1`和`func2`的函数对象，但不包含`base_func`。输出可能如下所示：

```python
[<function MyEnv.func1>, <function MyEnv.func2>]
```
## FunctionDef __get_defined_func_name__
**__get_defined_func_name__ 函数**: 此函数的功能是获取子类中所有的函数名称，排除以'_'字符开头的方法。

该函数`__get_defined_func_name__`是一个类方法，用于获取一个类中定义的所有公共函数名称。它首先调用另一个方法`__get_defined_func__`来获取类中定义的所有函数（包括私有和受保护的函数），然后通过过滤掉名称以单下划线'_'开头的函数，来确保只返回公共函数的名称。

具体步骤如下：
1. 调用`cls.__get_defined_func__()`获取类`cls`中定义的所有函数列表。
2. 使用`map`函数配合一个lambda表达式，将函数列表中每个函数对象转换为其名称字符串。
3. 将map对象转换为列表，以便返回一个包含函数名称的列表。

**注意**：
- 该函数假设`__get_defined_func__`方法存在于类中，并且能够返回一个包含函数对象的列表。
- 该函数只返回不以单下划线'_'开头的函数名称，通常这意味着只返回公共函数名称。
- 该函数返回的是一个字符串列表，每个字符串都是一个函数名称。

**输出示例**：
假设有一个类`ExampleClass`，它有三个方法：`public_method`, `_private_method`, `__magic_method__`。调用`ExampleClass.__get_defined_func_name__()`将返回以下列表：
```python
['public_method', '__magic_method__']
```
注意，`_private_method`因为以单下划线开头，所以被排除在外。
***
