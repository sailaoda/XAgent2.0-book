# AsyncFunctionDef chatcompletion_request
**chatcompletion_request函数**：该函数用于处理OpenAI v1.x.x聊天完成操作。

该函数通过提供的参数操作OpenAI v1.x.x聊天完成。它获取模型名称，并应用JSON Web令牌。如果响应表明上下文长度已超出限制，它将尝试获取更高容量的语言模型（如果在配置中存在），并重新尝试操作。否则，它将引发错误消息。

参数：
- max_lenght_fallback：如果为True，则在上下文长度超出限制时回退到更长的模型。
- **kwargs：可变长度的参数列表，包括（model:str等）。

返回值：
- response（dict）：包含来自Chat API的响应的字典。字典的结构基于API响应格式。

抛出异常：
- BadRequestError：如果在聊天完成操作期间发生任何错误，或者上下文长度超出限制且没有可回退的模型。

代码分析和描述：
- 首先，从kwargs中获取模型名称，默认为CONFIG.request.default_model。
- 然后，根据模型名称获取chatcompletion_kwargs，即配置中与模型相关的API配置。
- 根据chatcompletion_kwargs中的信息，创建AsyncOpenAI或AsyncAzureOpenAI的实例，用于与OpenAI API进行通信。
- 调用client.chat.completions.create方法，使用chatcompletion_kwargs作为参数，发起聊天完成操作。
- 获取聊天完成的响应，并将其转换为字典格式的response。
- 如果response中的第一个选择的finish_reason为"length"，则引发BadRequestError异常，表示上下文长度超出限制。
- 如果捕获到BadRequestError异常，并且错误消息中包含"maximum context length"，且max_lenght_fallback为True，则尝试使用更长的模型进行回退。
- 如果当前模型为"gpt-4"，并且在CONFIG.api_keys中存在"gpt-4-32k"或"gpt-3.5-turbo-16k"，则将模型名称更改为相应的模型。
- 否则，引发原始的BadRequestError异常。
- 记录日志，指示上下文长度超出限制，并回退到新的模型。
- 更新chatcompletion_kwargs中的模型名称，并通过递归调用chatcompletion_request函数重新尝试聊天完成操作。
- 如果捕获到其他类型的BadRequestError异常，则打印异常信息并重新引发异常。
- 返回聊天完成的响应。

注意事项：
- 该函数依赖于CONFIG模块中的配置信息，包括默认模型、API密钥等。
- 函数中使用了AsyncOpenAI和AsyncAzureOpenAI类，这些类用于与OpenAI API进行通信。
- 函数中使用了BadRequestError异常，用于处理聊天完成操作中的错误情况。

输出示例：
{
    "id": "chatcmpl-6p9XYPYSTTRi0xEviKjjilqrWU2Ve",
    "object": "chat.completion",
    "created": 1677649420,
    "model": "gpt-3.5-turbo",
    "usage": {
        "prompt_tokens": 56,
        "completion_tokens": 31,
        "total_tokens": 87
    },
    "choices": [
        {
            "message": {
                "role": "system",
                "content": "You are a helpful assistant."
            },
            "finish_reason": "stop",
            "index": 0
        }
    ]
}
***
# AsyncFunctionDef embedding_request
**embedding_request函数**: 该函数的功能是获取给定文本的嵌入向量。

该`embedding_request`函数是一个异步函数，用于将文本转换为嵌入向量，通常用于自然语言处理中的文本相似度比较、聚类等任务。函数接受一个字符串类型的参数`text`，这是需要转换为嵌入向量的文本。

函数首先创建一个`AsyncOpenAI`客户端实例，使用`CONFIG.request.ada_embedding.api_key`和`CONFIG.request.ada_embedding.base_url`作为配置参数。这些配置参数通常存储API密钥和基础URL，用于访问OpenAI提供的嵌入向量服务。

接着，函数从配置中获取嵌入模型的名称，存储在`embedding_model`变量中。然后，使用`recorder.log`记录使用的嵌入模型。

函数尝试调用`embedding_client.embeddings.create`方法，传入文本和模型名称，以获取文本的嵌入向量。如果请求成功，它将从响应中提取嵌入向量并返回。如果在请求过程中遇到`BadRequestError`错误，则会捕获该错误，并使用`recorder.log`记录错误信息，同时输出黄色的文字提醒。

**注意**:
- 由于`embedding_request`是一个异步函数，调用它时需要使用`await`关键字。
- 函数返回的嵌入向量是一个列表，其中包含了文本的嵌入表示。
- 如果在请求过程中发生错误，函数可能返回`None`或者不完整的数据，调用者需要对此进行检查和处理。
- `recorder.log`用于记录日志信息，需要确保`recorder`对象已经正确初始化并且可以使用。
- 函数中使用的`CONFIG`对象需要包含正确的配置信息，包括API密钥、基础URL和嵌入模型名称。

**输出示例**:
```python
[0.123, 0.456, 0.789, ..., 0.321]  # 假设的嵌入向量列表，实际的向量会包含更多的浮点数值
```
以上是一个模拟的返回值示例，实际返回的嵌入向量会根据所使用的模型和输入文本的不同而有所差异。
***
