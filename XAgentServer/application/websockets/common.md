# AsyncFunctionDef check_user
**check_user函数**：该函数用于检查WebSocket连接的用户。

该函数的作用是检查用户是否存在以及用户的有效性。首先，它会调用UserCRUD.is_exist()函数来检查用户是否存在，如果用户不存在，则会抛出XAgentWebSocketConnectError异常，提示用户不存在。接下来，它会调用UserCRUD.user_is_valid()函数来验证用户的有效性，如果用户无效，则会抛出XAgentWebSocketConnectError异常，提示用户无效。

然后，它会调用UserCRUD.get_user()函数来获取用户的详细信息，并进行一系列的判断。如果用户不存在、用户的token与传入的token不匹配、用户不可用或者用户不是beta用户，则会抛出XAgentWebSocketConnectError异常，提示用户连接失败。

**注意**：在使用该函数时，需要传入数据库连接对象（db）、用户ID（user_id）和令牌（token）。如果用户不存在或者用户无效，将会抛出XAgentWebSocketConnectError异常。
***
# FunctionDef handle_data
**handle_data函数**：handle_data函数的作用是处理websocket响应的数据。

该函数接受两个参数：row和root_dir。row是一个Raw对象，包含了原始数据；root_dir是一个字符串，表示根目录的路径。

函数首先从row中获取data字段的值，并将其赋给变量data。然后，函数尝试从data中获取using_tools字段的值，如果该字段不存在或为空，则直接返回data。

接下来，函数判断tool_name和tool_output字段的值。如果tool_name等于"PythonNotebook_execute_cell"，则遍历tool_output中的每个元素，如果元素是字典且包含'file_name'字段，则将file_name的值赋给变量file_name。然后，函数根据root_dir和file_name构建文件路径，并检查该路径是否存在。如果文件存在，则尝试以二进制方式打开文件，并将文件内容进行base64编码。最后，将编码后的内容赋给output字典的'file_data'字段，并将using_tools的'is_include_pictures'字段设置为True。

接着，函数处理tool_input字段。如果tool_input存在，则将其进行utf-8编码并解码为unicode_escape格式。

然后，函数处理tool_output字段。如果tool_output存在且为字符串类型，则将其进行utf-8编码并解码为unicode_escape格式。

最后，函数返回处理后的data。

**注意**：使用该代码时需要注意以下几点：
- 该函数依赖于Raw对象的结构，需要确保传入的row参数符合要求。
- 函数会根据tool_name字段的值来处理tool_output字段，需要确保这两个字段的值是一致的。

**输出示例**：模拟函数返回值的可能外观。
```python
{
    "using_tools": {
        "tool_name": "PythonNotebook_execute_cell",
        "tool_output": [
            {
                "file_name": "example.png",
                "file_data": "iVBORw0KGgoAAAANSUhEUgAA... (base64 encoded image data)"
            },
            ...
        ],
        "is_include_pictures": True
    },
    ...
}
```
***
# FunctionDef handle_workspace_filelist
**handle_workspace_filelist函数**：该函数的功能是处理工作空间文件列表。

该函数接受一个名为file_list的参数，它是一个文件名列表。

函数返回一个列表，列表中的每个元素都是一个字典，包含name和suffix两个键值对。

函数首先会检查file_list是否为列表类型且不为空，如果不满足条件，则返回一个空列表。

然后，函数会遍历file_list中的每个文件名，将文件名拆分成名称和后缀，并以字典的形式添加到结果列表中。

**注意**：使用该代码时需要注意以下几点：
- file_list参数必须是一个非空的列表。

**输出示例**：模拟代码返回值的可能外观。

```python
[{"name": "file1.txt", "suffix": "txt"}, {"name": "file2.py", "suffix": "py"}]
```

该函数在以下文件中被调用：
- XAgentServer/application/websockets/base.py
- XAgentServer/application/websockets/recorder.py
- XAgentServer/application/websockets/replayer.py
- XAgentServer/application/websockets/share.py

在这些文件中，函数被用于构造WebsocketResponseBody对象的workspace_file_list属性的值。
***
