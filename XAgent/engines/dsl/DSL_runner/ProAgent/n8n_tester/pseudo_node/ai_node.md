# FunctionDef run_ai_completion
**run_ai_completion函数**：该函数的功能是生成给定参数列表中每个参数的函数注释。

该函数接收一个参数列表`params_list`，该列表包含了多个参数字典，每个字典代表一次AI完成请求的参数。函数的主要流程如下：

1. 初始化一个空列表`return_list`，用于存储每次AI完成请求的结果。
2. 从配置中获取默认的AI完成请求参数`completion_kwargs`。
3. 遍历`params_list`中的每个参数字典：
   - 从字典中获取`messages`字段，该字段可能是一个字符串或列表。
   - 如果`messages`是字符串，则将其解析为JSON对象；如果是列表，则直接使用。
   - 调用`_chat_completion_request`函数，传入`messages_json`和其他默认参数，发起AI完成请求。
   - 从请求结果中提取内容`content`，并构造返回的JSON对象，添加到`return_list`中。
4. 最终返回`return_list`，其中包含了所有请求的AI完成结果。

**注意**：
- 函数中使用了`CONFIG.default_completion_kwargs`来获取默认的AI完成请求参数，这意味着在使用前必须确保`CONFIG`对象已经被正确初始化并包含了`default_completion_kwargs`属性。
- 函数内部调用了一个名为`_chat_completion_request`的函数，但在代码片段中并未提供该函数的实现，因此需要确保该函数在调用`run_ai_completion`之前已经被定义和可用。
- 函数返回的是一个列表，每个元素是一个包含特定结构的字典，这个结构需要与调用方的期望结构相匹配。

**输出示例**：
```python
[
    {
        "json": {
            'choices': [
                {
                    'text': '这里是AI生成的文本内容'
                }
            ]
        },
        "pairedItem": {
            "item": 0
        }
    },
    # ... 其他返回结果
]
```
在这个示例中，`return_list`包含了一个或多个字典，每个字典都有一个`json`键，其值是一个包含`choices`列表的字典，`choices`列表中的每个元素都是一个包含`text`键的字典，其值是AI生成的文本内容。同时，每个字典还包含一个`pairedItem`键，其值是一个字典，表示配对项。

在项目中的调用情况中，`run_ai_completion`函数被`run_pseudo_workflow`函数调用，用于处理类型为`aiCompletion`的节点。`run_pseudo_workflow`函数负责运行一个伪工作流，根据节点类型分别调用不同的处理函数，其中`aiCompletion`类型的节点就使用`run_ai_completion`函数来处理。处理完成后，将结果填充到最终的返回数据中。
***
