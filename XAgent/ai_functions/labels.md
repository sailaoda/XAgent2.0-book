# ClassDef AIFunctionLabel
**AIFunctionLabel功能**: 此类的功能是定义AI函数的元数据，包括函数的名称、描述、返回类型、参数模式和提示信息。

AIFunctionLabel类是一个用于存储和描述AI函数特征的数据结构。它包含以下属性：

- `name`: 字符串类型，表示AI函数的名称。
- `description`: 字符串类型，表示AI函数的描述。
- `return_type`: TypeAdapter类型，表示AI函数的返回值类型。
- `schema`: 字典类型，包含AI函数参数的模式。
- `prompt`: 字符串类型，提供给AI函数的提示信息，通常用于生成函数调用的文本提示。
- `completions_kwargs`: 字典类型，默认为空字典，用于存储执行AI函数时的额外关键字参数。

在项目中，AIFunctionLabel类被用于以下两种情况：

1. 在`function_manager.py`文件中，`execute_ai_function`异步方法接收一个AIFunctionLabel实例作为参数，根据其提供的元数据执行对应的AI函数。方法首先记录执行信息，然后根据AIFunctionLabel中的`prompt`和其他参数生成消息，最后调用工具完成函数执行并返回结果。

2. 在`wrapper.py`文件中，定义了一个装饰器，它会检查被装饰的函数是否具有`__ai_function_label__`属性。如果没有，装饰器会创建一个AIFunctionLabel实例，并将其作为函数的属性。这个实例包含了函数的名称、描述、返回类型、参数模式和文档字符串作为提示。然后，装饰器定义了一个异步的包装函数，它将调用`function_manager.execute_ai_function`来执行AI函数，并返回结果。

**注意**：
- 使用AIFunctionLabel时，需要确保所有AI函数都有返回类型注解，否则在`wrapper.py`中的装饰器会抛出异常。
- `prompt`属性通常来源于函数的文档字符串，它是执行AI函数时提供给用户的文本提示，应确保其准确性和可用性。
- `completions_kwargs`属性可以包含执行AI函数时需要的任何额外参数，这些参数会传递给工具执行函数。

**输出示例**：
```python
# 假设有一个AI函数的AIFunctionLabel实例
ai_function_label_example = AIFunctionLabel(
    name="calculate_sum",
    description="计算两个数字的和",
    return_type=TypeAdapter(int),
    schema={"type": "object", "properties": {"a": {"type": "number"}, "b": {"type": "number"}}},
    prompt="请计算两个数字 {a} 和 {b} 的和。",
    completions_kwargs={}
)

# AIFunctionLabel实例的输出可能如下所示
{
    "name": "calculate_sum",
    "description": "计算两个数字的和",
    "return_type": <TypeAdapter int>,
    "schema": {"type": "object", "properties": {"a": {"type": "number"}, "b": {"type": "number"}}},
    "prompt": "请计算两个数字 {a} 和 {b} 的和。",
    "completions_kwargs": {}
}
```
***
