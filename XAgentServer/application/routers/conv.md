# FunctionDef user_is_available
**user_is_available函数**：此函数的功能是检查用户是否可用。

该函数接受以下参数：
- user_id（必填）：用户ID，字符串类型。
- token（必填）：令牌，字符串类型。
- db（可选）：数据库会话，依赖项。

函数内部逻辑如下：
- 首先，检查user_id是否为空字符串，如果是，则抛出XAgentAuthError异常，提示"user_id为空！"。
- 然后，通过调用UserCRUD.is_exist函数检查数据库中是否存在该用户，如果不存在，则抛出XAgentAuthError异常，提示"用户不存在！"。
- 接着，通过调用UserCRUD.user_is_valid函数检查用户是否可用，如果不可用，则抛出XAgentAuthError异常，提示"用户不可用！"。
- 最后，返回user_id作为结果。

**注意**：在使用此函数时需要注意以下几点：
- 必须提供有效的user_id和token参数。
- 需要确保数据库中存在该用户，并且用户是可用的。

**输出示例**：假设user_id为"12345"，函数将返回"12345"作为结果。
***
# AsyncFunctionDef get_all_interactions
**get_all_interactions 函数**: 此函数的功能是获取指定用户的所有交互信息。

该函数`get_all_interactions`是一个异步函数，用于根据用户ID检索该用户的所有交互信息。它接受四个参数：`user_id`、`page_size`、`page_num`和`db`。

- `user_id`: 一个字符串参数，通过`Depends(user_is_available)`依赖注入得到，用于标识需要查询交互信息的用户。
- `page_size`: 一个整数参数，通过`Form(...)`获取，表示每页显示的交互记录数。
- `page_num`: 一个整数参数，同样通过`Form(...)`获取，表示要获取的页码。
- `db`: 一个`Session`类型的参数，通过`Depends(get_db)`依赖注入得到，表示数据库会话实例。

函数内部，首先调用`InteractionCRUD.search_interaction_by_user_id`方法，传入数据库会话`db`、用户ID`user_id`、页大小`page_size`和页码`page_num`作为参数，以检索用户的交互信息。

最后，函数返回一个`ResponseBody`对象，其中包含了检索到的数据（`data`），操作成功标志（`success=True`），以及操作成功的消息（`message="success"`）。

**注意**：
- 确保在调用此函数之前，用户已经通过`user_is_available`函数验证。
- `page_size`和`page_num`参数需要通过表单提交的方式传递，这通常在HTTP POST请求中完成。
- 数据库会话`db`必须是有效的会话实例，通常是通过依赖注入的方式在请求处理过程中创建和提供。

**输出示例**：
```json
{
  "data": [
    {
      "interaction_id": "123",
      "content": "用户交互内容示例",
      "timestamp": "2023-04-01T12:00:00Z"
    },
    {
      "interaction_id": "124",
      "content": "另一个用户交互内容示例",
      "timestamp": "2023-04-01T12:05:00Z"
    }
  ],
  "success": true,
  "message": "success"
}
```
在这个示例中，`data`字段是一个包含交互记录的列表，每个记录包括交互ID、内容和时间戳。`success`字段表示操作是否成功，而`message`字段提供了操作的反馈信息。
***
# FunctionDef init_conv_env
**init_conv_env函数**: 该函数的功能是初始化会话环境。

该函数`init_conv_env`用于初始化用户的会话环境。它接受两个参数：`user_id`和`db`。`user_id`是一个字符串类型的参数，通过`Depends(user_is_available)`依赖注入得到，用于验证用户是否可用。`db`是一个`Session`类型的参数，通过`Depends(get_db)`依赖注入得到，用于提供数据库会话。

函数首先尝试通过`InteractionCRUD.get_ready_interaction`方法获取当前用户的“准备就绪”的交互会话。如果没有找到现有的交互会话，函数将创建一个新的交互会话。它生成一个新的UUID作为交互会话ID，然后创建一个`InteractionBase`实例，其中包含交互会话的各种信息，如创建时间、描述、状态等，并将其保存到数据库中。

如果已经存在一个“准备就绪”的交互会话，函数将直接使用该会话的ID。

最后，函数返回一个`ResponseBody`实例，其中包含交互会话ID和当前时间戳（毫秒级），并标记操作成功。

**注意**：
- 确保在调用此函数之前，`user_is_available`和`get_db`这两个依赖项已经正确实现，以确保能够验证用户并获取数据库会话。
- 函数返回的`ResponseBody`对象应与前端约定的数据格式保持一致，以便前端可以正确处理返回的数据。

**输出示例**：
```json
{
  "data": {
    "id": "生成的交互会话ID",
    "t": "当前时间戳"
  },
  "success": true,
  "message": "success"
}
```
在此示例中，`id`字段将是一个新生成的UUID字符串，表示交互会话的唯一标识；`t`字段是调用函数时的当前时间戳（毫秒级）。`success`和`message`字段表示操作是否成功及相关信息。
***
# AsyncFunctionDef get_share_interactions
**get_share_interactions函数**: 该函数的功能是获取用户共享的所有交互信息。

该函数`get_share_interactions`是一个异步函数，用于根据用户ID检索用户共享的所有交互信息。它接受四个参数：`user_id`、`page_size`、`page_num`和`db`。函数返回一个`ResponseBody`对象，其中包含检索到的数据和执行状态。

- `user_id`: 一个字符串参数，通过`Depends(user_is_available)`依赖注入得到，用于验证用户是否可用。
- `page_size`: 一个整数参数，通过`Form(...)`从表单数据中获取，表示每页显示的交互信息数量。
- `page_num`: 一个整数参数，同样通过`Form(...)`从表单数据中获取，表示要检索的页码。
- `db`: 一个`Session`对象，通过`Depends(get_db)`依赖注入得到，表示当前的数据库会话。

函数内部调用了`InteractionCRUD.search_many_shared`方法，传入数据库会话`db`、页大小`page_size`和页码`page_num`作为参数，以检索共享的交互信息。检索结果被存储在变量`data`中。

最后，函数构造了一个`ResponseBody`对象，其中`data`字段包含了检索到的交互信息，`success`字段被设置为`True`，`message`字段包含了字符串"success"，表示操作成功。然后返回这个`ResponseBody`对象。

**注意**：
- 在使用这个函数时，需要确保用户验证依赖`user_is_available`和数据库会话获取依赖`get_db`已经正确实现。
- 由于该函数使用了异步编程模式，调用它的上下文也应该支持异步操作。
- `Form(...)`用于从表单中接收数据，确保调用此函数的请求中包含了正确的表单数据。

**输出示例**：
一个可能的`ResponseBody`对象的返回值示例如下：
```json
{
  "data": [
    {
      "interaction_id": "123",
      "content": "示例交互内容",
      "shared": true,
      // 其他交互信息字段
    },
    // 更多交互信息对象
  ],
  "success": true,
  "message": "success"
}
```
在这个示例中，`data`字段包含了一个交互信息对象的列表，每个对象包含了交互的详细信息。`success`和`message`字段表明操作执行成功。
***
# AsyncFunctionDef share_interaction
**share_interaction函数**: 该函数的功能是共享用户的交互记录。

该函数`share_interaction`主要用于将用户完成的交互记录打包并发送到指定的共享服务。函数接收三个参数：`user_id`表示用户的唯一标识，`interaction_id`表示交互记录的唯一标识，`db`表示数据库会话实例。函数的执行流程如下：

1. 首先，通过`user_is_available`依赖项检查用户是否可用。
2. 使用`InteractionCRUD.get_interaction`方法从数据库中获取指定`interaction_id`的交互记录。如果没有找到交互记录，则返回错误消息。
3. 检查交互记录是否已完成。如果未完成，则返回错误消息。
4. 通过`UserCRUD.get_user`方法获取用户信息，并从中提取用户名称。
5. 构建交互记录的本地存储路径，并检查是否存在对应的工作空间压缩文件。如果不存在，则创建一个新的压缩文件，并将工作空间目录中的文件添加到压缩文件中。
6. 使用`InteractionCRUD.search_many_raws`方法获取与交互记录相关的原始数据。
7. 构建共享数据，包括用户ID、用户名、用户令牌、交互记录详情和原始数据。
8. 将工作空间压缩文件读取为二进制数据，并与共享数据一起通过HTTP POST请求发送到共享服务的URL。
9. 如果请求成功，将返回的JSON数据转换为`ResponseBody`对象并返回。如果请求失败或发生异常，则返回错误消息。

**注意**：
- 在使用该函数时，需要确保`user_is_available`、`get_db`等依赖项已正确配置。
- 函数中涉及到的`InteractionCRUD`和`UserCRUD`是操作数据库的CRUD类，需要确保这些类的方法能够正确执行。
- 函数使用了`os`和`zipfile`模块来处理文件路径和文件压缩，需要确保服务器有相应的文件读写权限。
- 函数使用了`requests`库来发送HTTP请求，需要确保网络连接正常，并且共享服务的URL是可访问的。
- 函数中对异常进行了捕获，但在实际部署时应考虑更详细的异常处理策略。

**输出示例**：
假设共享成功，函数可能返回如下的`ResponseBody`对象示例：
```json
{
  "success": true,
  "message": "Interaction shared successfully.",
  "data": {
    "share_link": "http://example.com/shared/interaction/12345"
  }
}
```
如果共享失败，函数可能返回如下的`ResponseBody`对象示例：
```json
{
  "success": false,
  "message": "interaction is not finish!",
  "data": null
}
```
***
# FunctionDef community
**community函数**: 该函数的功能是处理社区分享交互记录的请求。

该函数`community`是一个API端点，用于处理用户通过x-agent.net平台分享交互记录的请求。它接收多个参数，包括用户ID、用户名、交互记录数据、原始数据、文件以及数据库会话。函数首先解析交互记录和原始数据，然后检查是否已经存在相同的分享记录。如果存在，会抛出异常。接着，它会检查原始数据中是否包含状态为完成（FINISHED）的节点，如果没有，则同样抛出异常。之后，函数会在本地存储中创建相应的目录，暂存并解压上传的文件。最后，它会创建交互记录和分享记录，并将原始数据插入数据库。

详细代码分析如下：

1. 函数接收的参数包括：
   - `user_id`: 用户ID，通过`Depends(user_is_available)`依赖注入得到。
   - `user_name`: 用户名，通过表单数据提交。
   - `interaction`: 交互记录的JSON字符串，通过表单数据提交。
   - `raws`: 原始数据的JSON字符串，通过表单数据提交。
   - `files`: 用户上传的文件，通过`UploadFile`接收。
   - `db`: 数据库会话，通过`Depends(get_db)`依赖注入得到。

2. 函数开始时，会解析`interaction`和`raws`参数为JSON对象。

3. 检查是否已经存在相同的分享记录，如果存在，则抛出`XAgentWebError`异常。

4. 遍历原始数据，检查是否包含状态为完成的节点，如果没有，则抛出`XAgentWebError`异常。

5. 创建本地存储目录，用于存放用户上传的文件。

6. 将上传的文件写入到一个临时的zip文件中，并解压到指定目录。

7. 删除临时的zip文件。

8. 使用交互记录数据创建`InteractionBase`对象。

9. 创建`SharedInteractionBase`对象，用于记录分享信息。

10. 在数据库中创建交互记录和添加分享记录。

11. 遍历原始数据，对于数据库中不存在的节点，创建`XAgentRaw`对象并插入数据库。

12. 函数返回一个响应体，包含操作成功的信息。

**注意**:
- 在使用此函数时，需要确保传入的参数格式正确，特别是`interaction`和`raws`参数需要是有效的JSON字符串。
- 上传的文件需要是zip格式，并且函数会在处理过程中将其解压到本地存储目录。
- 函数中的异常处理确保了只有满足条件的交互记录才能被分享。

**输出示例**:
```json
{
  "data": null,
  "success": true,
  "message": "success"
}
```
以上是对`community`函数的详细说明文档。
***
# AsyncFunctionDef delete_interaction
**delete_interaction函数**: 该函数的功能是删除指定的交互记录。

该`delete_interaction`函数是一个异步函数，用于删除用户的交互记录。它接收三个参数：`user_id`、`interaction_id`和`db`。函数的具体分析如下：

1. `user_id: str = Depends(user_is_available)`：这是一个路径参数，通过依赖注入的方式获取当前用户的ID。`Depends(user_is_available)`表示在调用该函数之前，会先执行`user_is_available`函数来确保用户是有效的。

2. `interaction_id: str = Form(...)`：这是一个表单参数，用来接收要删除的交互记录的ID。`Form(...)`表示这是一个必填参数。

3. `db: Session = Depends(get_db)`：这是一个依赖注入参数，通过`Depends(get_db)`获取数据库会话对象，用于执行数据库操作。

函数内部逻辑如下：

- 首先，调用`InteractionCRUD.delete_interaction`方法，传入`db`和`interaction_id`参数，执行删除交互记录的操作。
- 然后，构造一个`ResponseBody`对象，其中`data`字段存放删除操作的结果，`success`字段设置为`True`，`message`字段设置为`"success"`，表示操作成功。

**注意**：
- 在使用该函数时，需要确保传入的`user_id`和`interaction_id`是有效且正确的。
- 该函数需要在异步环境下运行，因为它是一个`async`函数。
- 删除操作会影响数据库中的数据，因此在调用该函数之前应该进行必要的权限检查和确认操作。

**输出示例**：
```json
{
  "data": null,
  "success": true,
  "message": "success"
}
```
在这个示例中，`data`字段为`null`，因为删除操作通常不需要返回具体的数据内容。`success`字段为`true`，表示删除操作成功执行。`message`字段为`"success"`，提供了操作成功的额外信息。
***
# AsyncFunctionDef update_interaction_parameter
**update_interaction_parameter函数**: 此函数的功能是更新交互参数。

该函数`update_interaction_parameter`是一个异步函数，用于更新用户交互的参数。它接收多个参数，包括用户ID、模式、代理、文件列表和交互ID，以及数据库会话对象。函数首先检查交互ID是否为空，如果为空，则返回一个错误消息。然后，它尝试从数据库中获取与提供的交互ID相对应的交互对象。如果找不到交互对象，它将返回一个错误消息。如果找到了交互对象，函数将解析文件列表中的每个字符串为JSON对象，并更新交互的参数。最后，它返回一个包含更新数据的响应体。

详细代码分析：
- `user_id: str = Depends(user_is_available)`: 通过依赖注入获取当前可用的用户ID。
- `mode: str = Form(...)`: 从表单数据中获取模式字符串。
- `agent: str = Form(...)`: 从表单数据中获取代理字符串。
- `file_list: List[str] = Form(...)`: 从表单数据中获取文件列表，这是一个字符串列表。
- `interaction_id: str = Form(...)`: 从表单数据中获取交互ID字符串。
- `db: Session = Depends(get_db)`: 通过依赖注入获取数据库会话对象。

函数逻辑：
1. 检查`interaction_id`是否为空，如果为空，返回错误消息。
2. 使用`InteractionCRUD.get_interaction`方法尝试从数据库中获取交互对象。
3. 如果没有找到交互对象，返回错误消息。
4. 将文件列表中的每个字符串解析为JSON对象，并准备更新数据。
5. 使用`InteractionCRUD.update_interaction`方法更新数据库中的交互对象。
6. 返回一个包含更新数据的成功响应体。

**注意**：
- 在调用此函数之前，确保传入的`interaction_id`是有效的，否则会返回错误。
- `file_list`参数应该是一个有效的JSON字符串列表，函数将尝试将其解析为JSON对象。
- 此函数是异步的，应在异步环境中调用。

**输出示例**：
如果更新成功，可能的返回值如下：
```json
{
  "data": {
    "interaction_id": "123456",
    "agent": "agent_name",
    "mode": "edit",
    "file_list": [{"file_name": "file1.txt"}, {"file_name": "file2.txt"}]
  },
  "success": true,
  "message": "success!"
}
```
如果`interaction_id`为空或找不到交互对象，可能的返回值如下：
```json
{
  "success": false,
  "message": "interaction_id is empty!" 或 "Don't find any interaction by interaction_id: 123456, Please check your interaction_id!"
}
```
***
# AsyncFunctionDef update_interaction_description
**update_interaction_description函数**: 该函数的功能是更新交互描述。

该函数`update_interaction_description`是一个异步函数，用于更新用户交互的描述信息。它接受四个参数：`user_id`、`description`、`interaction_id`和`db`。其中`user_id`是通过`Depends(user_is_available)`依赖注入得到的，表示当前可用的用户ID；`description`和`interaction_id`是通过表单数据提交的，分别代表新的描述信息和交互的唯一标识；`db`是数据库会话实例，通过`Depends(get_db)`依赖注入得到。

函数首先检查`interaction_id`是否为空，如果为空，则返回一个包含错误信息的`ResponseBody`对象。接着，函数尝试通过`InteractionCRUD.get_interaction`方法从数据库中获取对应的交互实例。如果没有找到对应的交互实例，则返回一个包含错误信息的`ResponseBody`对象。

如果找到了对应的交互实例，函数将创建一个包含`interaction_id`和`description`的字典`update_data`。如果传入的`description`为空，则默认使用`"XAgent"`作为描述。然后，使用`InteractionCRUD.update_interaction`方法更新数据库中的交互实例。

最后，函数返回一个包含更新后的数据、成功标志和消息的`ResponseBody`对象。

**注意**：
- 确保在调用此函数之前，表单数据已正确提交，并且数据库会话实例`db`已经通过依赖注入得到。
- 在实际应用中，需要确保`user_id`对应的用户确实存在且可用。
- 如果`interaction_id`为空或者无效，函数将不会执行更新操作，并返回错误信息。

**输出示例**：
如果更新成功，可能的返回值示例为：
```json
{
  "data": {
    "interaction_id": "123456",
    "description": "新的描述信息"
  },
  "success": true,
  "message": "success!"
}
```
如果`interaction_id`为空，可能的返回值示例为：
```json
{
  "success": false,
  "message": "interaction_id is empty!"
}
```
如果没有找到对应的交互实例，可能的返回值示例为：
```json
{
  "success": false,
  "message": "Don't find any interaction by interaction_id: 123456, Please check your interaction_id!"
}
```
***
