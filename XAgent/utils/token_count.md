# FunctionDef get_token_nums
**get_token_nums函数**：该函数用于计算给定文本中的标记数量。

该函数接受一个字符串类型的参数text，表示需要计算标记数量的文本。

函数返回一个整数，表示文本中的标记数量。

该函数的具体实现是通过调用encoding.encode函数将文本转换为编码后的字符串，然后使用len函数计算编码后字符串的长度来得到标记数量。

**注意**：在使用该函数时，需要确保传入的参数text是一个字符串类型的文本。

**输出示例**：假设传入的文本为"Hello, world!"，则函数的返回值为3。
***
# FunctionDef clip_text
**clip_text函数**：clip_text函数的功能是将给定的文本截断为指定数量的标记。如果原始文本和截断后的文本长度不一致，则在截断后的文本的开头或结尾添加"`wrapped`"。

参数：
- text (str)：要截断的文本。
- max_tokens (int, 可选)：最大标记数。文本将被截断为不超过此数量的标记。
- clip_end (bool, 可选)：如果为True，则从文本末尾截断。如果为False，则从文本开头截断。

返回值：
- str：截断后的文本。
- int：原始文本中的标记总数。

clip_text函数首先将text转换为字符串类型，并对其进行编码。如果max_tokens不为None且小于等于0，则返回空字符串和0。否则，根据clip_end的值，从编码后的文本的开头或结尾截取max_tokens个标记，并将其解码为字符串。如果解码后的文本长度与原始文本长度不相等，则在解码后的文本的开头或结尾添加"`wrapped`"。最后，返回解码后的文本和编码后的文本的标记总数。

在XAgent/agent/base.py文件中，clip_text函数被用于截断工具调用的描述。在get_history_message函数中，根据配置的summary选项，获取历史消息并构造历史消息的概述。在构造历史消息时，使用clip_text函数对工具调用的描述进行截断，以确保消息的总标记数不超过指定的最大标记数。

在XAgent/agent/summarize.py文件中，clip_text函数被用于对工具调用的参数进行截断。在summarize_tool_calls函数中，根据工具调用的参数长度对参数进行排序，并使用clip_text函数对参数进行截断，以确保参数的总标记数不超过指定的最大标记数。

在XAgent/ai_functions/function_manager.py文件中，clip_text函数被用于构造AI函数的提示信息。在execute函数中，根据AI函数的配置和参数构造提示信息，并使用clip_text函数对提示信息进行截断，以确保提示信息的总标记数不超过指定的最大标记数。

在XAgent/engines/base.py文件中，clip_text函数被用于获取文件系统的结构。在get_file_system_structure函数中，调用ToolServerInterface的execute方法获取文件系统的结构，并使用clip_text函数对文件系统的结构进行截断，以确保文件系统结构的总标记数不超过指定的最大标记数。

在XAgent/models/toolcall.py文件中，clip_text函数被用于将工具调用的参数转换为字符串。在_wrap_arguments方法中，根据参数的长度对参数进行排序，并使用clip_text函数对参数进行截断，以确保参数的总标记数不超过指定的最大标记数。最后，将截断后的参数拼接为字符串。

**注意**：clip_text函数用于截断文本，以确保文本的总标记数不超过指定的最大标记数。

**输出示例**：
截断前的文本："这是一个测试文本，用于测试clip_text函数的功能。"
截断后的文本："这是一个测试文本，用于测试clip_text函数的功能。"
标记总数：16
***
