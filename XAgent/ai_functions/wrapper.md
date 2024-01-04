# FunctionDef ai_function
**ai_function 函数**: 此函数的功能是为其他函数提供一个装饰器，用于将它们标记为AI函数，并处理它们的执行。

ai_function 函数接受一个字典类型的参数 `completions_kwargs` 和任意数量的关键字参数 `**kwargs`。它返回一个装饰器，该装饰器用于装饰实际的AI函数。

详细代码分析如下：

1. `ai_function` 函数定义了一个内部的装饰器函数 `decorator`。
2. 当装饰器被应用到某个函数 `func` 上时，首先检查该函数是否已经有 `__ai_function_label__` 属性，这个属性表示函数是否已经被标记为AI函数。
3. 如果函数 `func` 没有 `__ai_function_label__` 属性，装饰器将进行以下步骤：
   - 使用 `inspect.signature(func)` 获取函数的签名，并检查函数是否有返回类型注解。如果没有，将抛出 `ValueError` 异常。
   - 使用 `TypeAdapter` 类将返回类型注解转换为相应的类型，并生成 JSON Schema。
   - 从返回类型的 JSON Schema 中提取描述信息，并将函数的文档字符串作为提示信息 `prompt`。
   - 创建一个 `AIFunctionLabel` 实例，包含函数的名称、描述、返回类型、Schema 和提示信息等，并将其设置为函数的 `__ai_function_label__` 属性。
4. 装饰器内部定义了一个异步的 `wrapper` 函数，它将被用来执行AI函数。
   - `wrapper` 函数将接收任意数量的位置参数 `*args` 和关键字参数 `**kwargs`。
   - `wrapper` 函数将所有的关键字参数值转换为字符串类型。
   - 使用 `function_manager.execute_ai_function` 方法执行AI函数，并传入 `__ai_function_label__` 属性和所有参数。
   - 返回执行结果。
5. 最终，装饰器返回 `wrapper` 函数。

**注意**：
- 使用此装饰器的函数必须包含返回类型注解，否则将抛出异常。
- 被装饰的函数的文档字符串将被用作提示信息，因此应确保其提供了有用的描述。
- `completions_kwargs` 参数用于传递给AI函数执行时的额外参数，这些参数将在执行AI函数时被使用。

**输出示例**：
由于 `ai_function` 是一个装饰器，它本身不直接返回结果，而是返回一个包装了原函数的新函数。因此，输出示例将取决于被装饰的函数的返回值。例如，如果被装饰的函数返回一个字符串 "Hello, AI!"，那么使用此装饰器后的函数也将返回 "Hello, AI!"。
## FunctionDef decorator
**decorator函数**: 此函数的作用是将一个普通函数转换为一个异步AI函数，并为其附加一个特殊的标签属性。

decorator函数是一个高阶函数，它接受一个函数作为参数，并返回一个新的异步函数（称为wrapper）。这个新函数在调用时，会执行一些额外的处理，然后调用原始函数，并返回原始函数的结果。

具体来说，decorator函数首先检查传入的函数（func）是否已经有一个名为"__ai_function_label__"的属性。如果没有，decorator函数会进行以下操作：
1. 获取func的名称（name）。
2. 使用`inspect.signature`获取func的返回类型注解（return_type）。如果func没有返回类型注解，则抛出一个ValueError异常。
3. 将返回类型注解转换为一个TypeAdapter对象，并从中生成JSON模式（ret_schema）。
4. 从ret_schema中提取描述信息（description），并获取func的文档字符串（prompt）作为提示信息。
5. 创建一个AIFunctionLabel对象，包含函数名称、描述、返回类型、JSON模式和提示信息，并将其设置为func的"__ai_function_label__"属性。

在func被装饰后，调用wrapper函数时，它会将所有关键字参数（kwargs）的值转换为字符串，然后调用function_manager.execute_ai_function来执行AI函数，并返回执行结果。

在项目中，decorator函数被用在ai_function装饰器中，ai_function装饰器接受一个字典类型的completions_kwargs参数和其他关键字参数，然后返回decorator函数。这样，使用ai_function装饰器的函数会自动被转换为异步AI函数，并且具有AI功能的标签属性。

**注意**：
- 使用decorator函数时，需要确保传入的函数具有返回类型注解，否则会抛出异常。
- 由于wrapper函数是异步的，因此在调用被decorator装饰的函数时，需要使用`await`关键字。

**输出示例**：
假设有一个如下所示的被decorator装饰的函数：

```python
@ai_function(completions_kwargs={"max_tokens": 100})
async def example_function(param1: str) -> str:
    """这是一个示例函数。"""
    return f"处理后的参数: {param1}"
```

调用此函数并等待结果可能会返回如下：

```python
result = await example_function("测试参数")
print(result)  # 输出: 处理后的参数: 测试参数
```
### AsyncFunctionDef wrapper
**wrapper函数**: 此函数的功能是执行异步的AI功能，并返回执行结果。

wrapper函数是一个异步函数，它的主要作用是对传入的函数进行包装，使其能够异步执行AI相关的功能。在XAgent项目中，wrapper函数被用作装饰器，用于处理AI功能的执行流程。

具体来说，wrapper函数接受任意数量的位置参数(*args)和关键字参数(**kwargs)。在函数体内，它首先将所有的关键字参数值转换为字符串类型，这是因为在执行AI功能时，通常需要将参数序列化为可以通过网络传输的格式。

随后，wrapper函数通过`function_manager.execute_ai_function`方法异步执行AI功能。这个方法接受一个标签（通过`getattr(func, "__ai_function_label__")`获取）和之前处理过的参数。这个标签是在装饰器中创建并附加到原函数上的，它包含了原函数的名称、描述、返回类型、JSON模式和文档字符串等信息。

最后，wrapper函数返回AI功能执行的结果。

在项目中，wrapper函数被用作装饰器来装饰那些被标记为AI功能的函数。装饰器首先检查目标函数是否已经有`__ai_function_label__`属性，如果没有，则创建一个新的AIFunctionLabel实例，并将其作为`__ai_function_label__`属性附加到目标函数上。这个过程中，装饰器还会检查函数的返回类型注解，并生成相应的JSON模式。

**注意**：
- 使用wrapper函数时，需要确保目标函数有返回类型注解，否则会抛出`ValueError`。
- wrapper函数是异步的，因此在调用时需要使用`await`关键字。
- wrapper函数和装饰器一起使用时，会改变原函数的签名，因为它会返回一个新的异步函数。

**输出示例**：
假设有一个AI功能函数`async def predict(text: str) -> float`，它被wrapper装饰器装饰后，调用此函数可能会返回一个浮点数，例如`0.85`，表示预测结果。
***
