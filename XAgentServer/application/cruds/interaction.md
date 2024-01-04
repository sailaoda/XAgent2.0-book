# ClassDef InteractionCRUD
**InteractionCRUD函数**: 这个类的功能是交互CRUD（增删改查）操作。

这个类是一个抽象基类（Abstract Base Class），用于定义交互CRUD操作的接口。它包含了一系列的类方法，用于对交互数据进行增删改查的操作。

- `search_many_interaction(db: Session) -> list`: 查询多个交互数据。该方法接受一个数据库会话对象作为参数，并返回一个包含多个交互数据的列表。

- `get_interaction(db: Session, interaction_id: str) -> InteractionBase | None`: 获取指定交互数据。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，并返回一个交互数据对象。如果找不到指定的交互数据，则返回None。

- `create_interaction(db: Session, base: InteractionBase)`: 创建交互数据。该方法接受一个数据库会话对象和一个交互数据对象作为参数，用于将交互数据保存到数据库中。

- `get_ready_interaction(db: Session, user_id: str)`: 获取准备就绪的交互数据。该方法接受一个数据库会话对象和一个用户ID作为参数，并返回一个准备就绪的交互数据对象。

- `add_parameter(db: Session, parameter: InteractionParameter = None)`: 添加参数。该方法接受一个数据库会话对象和一个参数对象作为参数，用于将参数添加到交互数据中。

- `get_parameter(db: Session, interaction_id: str) -> list`: 获取参数。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，并返回一个包含多个参数的列表。

- `get_init_parameter(db: Session, interaction_id: str) -> InteractionParameter`: 获取初始参数。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，并返回一个初始参数对象。

- `search_interaction_by_user_id(db: Session, user_id: str, page_size: int = 10, page_num: int = 1) -> list[dict]`: 根据用户ID查询交互数据。该方法接受一个数据库会话对象、一个用户ID和分页参数作为参数，并返回一个包含多个交互数据的列表。

- `is_exist(db: Session, interaction_id: str) -> bool`: 判断交互数据是否存在。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，并返回一个布尔值，表示交互数据是否存在。

- `update_interaction(db: Session, base_data: dict)`: 更新交互数据。该方法接受一个数据库会话对象和一个包含更新数据的字典作为参数，用于更新交互数据。

- `update_interaction_status(db: Session, interaction_id: str, status: str, message: str, current_step: int)`: 更新交互数据的状态。该方法接受一个数据库会话对象、一个交互数据的ID、状态、消息和当前步骤作为参数，用于更新交互数据的状态。

- `update_interaction_parameter(db: Session, interaction_id: str, parameter: InteractionParameter)`: 更新交互数据的参数。该方法接受一个数据库会话对象、一个交互数据的ID和一个参数对象作为参数，用于更新交互数据的参数。

- `is_running(db: Session, user_id: str)`: 判断用户是否正在运行交互数据。该方法接受一个数据库会话对象和一个用户ID作为参数，并返回一个布尔值，表示用户是否正在运行交互数据。

- `delete_interaction(db: Session, interaction_id: str)`: 删除交互数据。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，用于删除交互数据。

- `get_shared_interaction(db: Session, interaction_id: str) -> InteractionBase | None`: 获取共享的交互数据。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，并返回一个共享的交互数据对象。如果找不到指定的共享交互数据，则返回None。

- `search_many_shared(db: Session, page_size: int = 20, page_index: int = 1) -> list[dict]`: 查询多个共享的交互数据。该方法接受一个数据库会话对象和分页参数作为参数，并返回一个包含多个共享交互数据的列表。

- `insert_raw(db: Session, process: XAgentRaw)`: 插入原始数据。该方法接受一个数据库会话对象和一个原始数据对象作为参数，用于将原始数据保存到数据库中。

- `search_many_raws(db: Session, interaction_id: str) -> List[XAgentRaw] | None`: 查询多个原始数据。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，并返回一个包含多个原始数据的列表。

- `get_raw(db: Session, interaction_id: str, node_id: str) -> XAgentRaw | None`: 获取原始数据。该方法接受一个数据库会话对象、一个交互数据的ID和一个节点ID作为参数，并返回一个原始数据对象。如果找不到指定的原始数据，则返回None。

- `get_next_send(db: Session, interaction_id: str) -> List[Raw] | None`: 获取下一个发送的原始数据。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，并返回一个包含多个原始数据的列表。

- `update_send_flag(db: Session, interaction_id: str, node_id: str)`: 更新发送标志。该方法接受一个数据库会话对象、一个交互数据的ID和一个节点ID作为参数，用于更新发送标志。

- `update_receive_flag(db: Session, interaction_id: str, node_id: str)`: 更新接收标志。该方法接受一个数据库会话对象、一个交互数据的ID和一个节点ID作为参数，用于更新接收标志。

- `update_human_data(db: Session, interaction_id: str, node_id: str, human_data: dict)`: 更新人工数据。该方法接受一个数据库会话对象、一个交互数据的ID、一个节点ID和一个人工数据字典作为参数，用于更新人工数据。

- `insert_error(db: Session, interaction_id: str, message: str, status: str = "failed")`: 插入错误数据。该方法接受一个数据库会话对象、一个交互数据的ID、一个错误消息和一个状态（默认为"failed"）作为参数，用于插入错误数据。

- `add_share(db: Session, share)`: 添加共享。该方法接受一个数据库会话对象和一个共享对象作为参数，用于添加共享。

- `get_finish_status(db: Session, interaction_id: str) -> bool`: 获取完成状态。该方法接受一个数据库会话对象和一个交互数据的ID作为参数，并返回一个布尔值，表示交互数据是否已完成。

**注意**: 使用该类时需要注意以下几点：
- 需要传入一个有效的数据库会话对象作为参数。
- 部分方法可能会抛出XAgentDBError异常，需要进行异常处理。

**输出示例**:
```python
# 查询多个交互数据
interactions = InteractionCRUD.search_many_interaction(db)
print(interactions)
# 输出: [{'id': '1', 'name': 'interaction1'}, {'id': '2', 'name': 'interaction2'}, ...]

# 获取指定交互数据
interaction = InteractionCRUD.get_interaction(db, '1')
print(interaction)
# 输出: {'id': '1', 'name': 'interaction1'}

# 创建交互数据
base = InteractionBase(id='3', name='interaction3')
InteractionCRUD.create_interaction(db, base)

# 获取准备就绪的交互数据
ready_interaction = InteractionCRUD.get_ready_interaction(db, 'user1')
print(ready_interaction)
# 输出: {'id': '1', 'name': 'interaction1'}

# 添加参数
parameter = InteractionParameter(name='param1', value='value1')
InteractionCRUD.add_parameter(db, parameter)

# 获取参数
parameters = InteractionCRUD.get_parameter(db, '1')
print(parameters)
# 输出: [{'name': 'param1', 'value': 'value1'}, {'name': 'param2', 'value': 'value2'}, ...]

# 获取初始参数
init_parameter = InteractionCRUD.get_init_parameter(db, '1')
print(init_parameter)
# 输出: {'name': 'param1', 'value': 'value1'}

# 根据用户ID查询交互数据
interactions = InteractionCRUD.search_interaction_by_user_id(db, 'user1', page_size=10, page_num=1)
print(interactions)
# 输出: [{'id': '1', 'name': 'interaction1'}, {'id': '2', 'name': 'interaction2'}, ...]

# 判断交互数据是否存在
exist = InteractionCRUD.is_exist(db, '1')
print(exist)
# 输出: True

# 更新交互数据
base_data = {'id': '1', 'name': 'new_interaction'}
InteractionCRUD.update_interaction(db, base_data)

# 更新交互数据的状态
InteractionCRUD.update_interaction_status(db, '1', 'running', 'processing', 2)

# 更新交互数据的参数
InteractionCRUD.update_interaction_parameter(db, '1', parameter)

# 判断用户是否正在运行交互数据
running = InteractionCRUD.is_running(db, 'user1')
print(running)
# 输出: True

# 删除交互数据
InteractionCRUD.delete_interaction(db, '1')

# 获取共享的交互数据
shared_interaction = InteractionCRUD.get_shared_interaction(db, '1')
print(shared_interaction)
# 输出: {'id': '1', 'name': 'shared_interaction'}

# 查询多个共享的交互数据
shared_interactions = InteractionCRUD.search_many_shared(db, page_size=20, page_index=1)
print(shared_interactions)
# 输出: [{'id': '1', 'name': 'shared_interaction1'}, {'id': '2', 'name': 'shared_interaction2'}, ...]

# 插入原始数据
raw_data = XAgentRaw(id='1', data='raw_data')
InteractionCRUD.insert_raw(db, raw_data)

# 查询多个原始数据
raws = InteractionCRUD.search_many_raws(db, '1')
print(raws)
# 输出: [{'id': '1', 'data': 'raw_data1'}, {'id': '2', 'data': 'raw_data2'}, ...]

# 获取原始数据
raw = InteractionCRUD.get_raw(db, '1', '1')
print(raw)
# 输出: {'id': '1', 'data': 'raw_data1'}

# 获取下一个发送的原始数据
next_send = InteractionCRUD.get_next_send(db, '1')
print(next_send)
# 输出: [{'id': '1', 'data': 'raw_data1'}, {'id': '2', 'data': 'raw_data2'}, ...]

# 更新发送标志
InteractionCRUD.update_send_flag(db, '1', '1')

# 更新接收标志
InteractionCRUD.update_receive_flag(db, '1', '1')

# 更新人工数据
human_data = {'name': 'John', 'age': 30}
InteractionCRUD.update_human_data(db, '1', '1', human_data)

# 插入错误数据
InteractionCRUD.insert_error(db, '1', 'error message')

# 添加共享
share = {'id': '1', 'name': 'shared_interaction'}
InteractionCRUD.add_share(db, share)

# 获取完成状态
finish_status = InteractionCRUD.get_finish_status(db, '1')
print(finish_status)
# 输出: True
```
## FunctionDef search_many_interaction
**search_many_interaction函数**: 该函数的功能是搜索多个交互记录。

该函数`search_many_interaction`定义在XAgentServer的CRUD（创建、读取、更新、删除）模块中，用于从数据库中检索多个交互记录。它接收一个参数`db`，这是一个`Session`对象，用于与数据库进行会话。

函数体内部，它尝试调用`InteractionDBInterface`类的`search_many_interaction`方法，并将`db`作为参数传递。这个方法负责执行实际的数据库查询操作，返回查询结果。

如果在执行查询过程中发生任何异常，函数将捕获这个异常，并抛出一个`XAgentDBError`错误。这个错误是自定义的，用于表示在与数据库交互模块中发生了错误。异常信息会被格式化并包含在错误消息中，以便于调试和错误追踪。

**注意**：
- 使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- 在调用此函数时，应当处理可能抛出的`XAgentDBError`异常，以便于在出现数据库错误时进行适当的错误处理。
- 由于该函数依赖于`InteractionDBInterface`类的实现，因此需要确保该类的`search_many_interaction`方法能够正确执行并返回预期的结果。

**输出示例**：
调用`search_many_interaction`函数可能会返回如下格式的列表：
```python
[
    {
        "id": 1,
        "user_id": "user123",
        "session_id": "session456",
        "interaction_data": "Some interaction data",
        ...
    },
    {
        "id": 2,
        "user_id": "user789",
        "session_id": "session101",
        "interaction_data": "Some other interaction data",
        ...
    },
    ...
]
```
这个列表包含了多个字典，每个字典代表一个交互记录的详细信息。具体的字段取决于`InteractionDBInterface`类的`search_many_interaction`方法如何定义返回的数据结构。
## FunctionDef get_interaction
**get_interaction函数**：该函数的功能是获取交互信息。

该函数接受以下参数：
- db：数据库会话对象
- interaction_id：交互ID

该函数返回一个InteractionBase对象或None。

该函数可能会抛出XAgentDBError异常，表示XAgent数据库错误。

该函数的主要功能是通过调用InteractionDBInterface的get_interaction方法，从数据库中获取指定交互ID的交互信息。如果获取失败，会抛出XAgentDBError异常。

在项目中，该函数被以下文件调用：
- XAgentServer/application/routers/conv.py文件中的share_interaction函数
- XAgentServer/application/routers/conv.py文件中的update_interaction_parameter函数
- XAgentServer/application/routers/conv.py文件中的update_interaction_description函数
- XAgentServer/application/routers/workspace.py文件中的modify_upload_file函数
- XAgentServer/application/routers/workspace.py文件中的file函数
- XAgentServer/application/websockets/base.py文件中的dispatch方法
- XAgentServer/application/websockets/base.py文件中的on_connect方法
- XAgentServer/application/websockets/base.py文件中的task_handler方法
- XAgentServer/application/websockets/recorder.py文件中的task_handler方法

**注意**：在使用该函数时，需要确保传入正确的交互ID，并处理可能抛出的XAgentDBError异常。

**输出示例**：返回一个InteractionBase对象或None。
## FunctionDef create_interaction
**create_interaction函数**：此函数的功能是创建交互。

该函数用于在数据库中创建一个新的交互记录。它接受两个参数：db和base。db是数据库会话对象，base是一个包含交互基本信息的对象。

在函数内部，它首先尝试调用InteractionDBInterface的create_interaction方法来创建交互记录。如果出现异常，它会抛出XAgentDBError并附带错误信息。

此函数在以下文件中被调用：
- 文件路径：XAgentServer/application/routers/conv.py
  调用部分代码：
  ```
  InteractionCRUD.create_interaction(db=db, base=base)
  ```
- 文件路径：XAgentServer/application/websockets/recorder.py
  调用部分代码：
  ```
  InteractionCRUD.create_interaction(db=self.db, base=base)
  ```

**注意**：在使用此函数时需要注意以下几点：
- 需要提供有效的数据库会话对象（db）和交互基本信息（base）。
- 如果在创建交互记录时出现异常，将抛出XAgentDBError异常。
- 此函数在多个文件中被调用，确保在调用之前已经导入了相关的模块和类。
## FunctionDef get_ready_interaction
**get_ready_interaction函数**: 此函数的作用是获取准备就绪的交互会话。

`get_ready_interaction`函数是`InteractionCRUD`类的一个方法，它的主要作用是从数据库中获取一个状态为准备就绪（ready）的交互会话。如果没有找到准备就绪的会话，那么在调用的地方可能会创建一个新的交互会话。

详细代码分析如下：
- 函数接受两个参数：`db`和`user_id`。`db`是一个数据库会话对象，用于执行数据库操作；`user_id`是一个字符串，表示用户的唯一标识。
- 函数内部，首先尝试调用`InteractionDBInterface.get_ready_interaction`方法，传入`db`和`user_id`参数，以获取准备就绪的交互会话。
- 如果在尝试过程中发生任何异常，函数将捕获这个异常，并抛出一个`XAgentDBError`错误，错误信息中包含了异常的详细描述。

在项目中的调用情况：
- 在`XAgentServer/application/routers/conv.py`文件中，`init_conv_env`函数中调用了`get_ready_interaction`函数。该函数用于初始化会话环境。
- 如果`get_ready_interaction`没有找到准备就绪的交互会话，`init_conv_env`函数将创建一个新的交互会话，并将其保存到数据库中。
- 如果找到了准备就绪的交互会话，`init_conv_env`函数将使用该会话的`interaction_id`。

**注意**：
- 使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，且`user_id`是正确的用户标识。
- 在处理数据库操作时，要注意异常处理，确保在数据库操作失败时能够给出清晰的错误信息。

**输出示例**：
如果函数成功找到一个准备就绪的交互会话，它可能返回如下的对象：
```python
{
    "interaction_id": "1234567890abcdef",
    "user_id": "user123",
    "create_time": "2023-04-01 12:00:00",
    "description": "XAgent",
    "agent": "",
    "mode": "",
    "file_list": [],
    "recorder_root_dir": "",
    "status": "ready",
    "message": "ready...",
    "current_step": "-1",
    "update_time": "2023-04-01 12:00:00"
}
```
如果没有找到准备就绪的交互会话，函数可能会触发创建新会话的流程，但本身不会直接返回新创建的会话对象。
## FunctionDef add_parameter
**add_parameter 函数**: 此函数的功能是向数据库中添加一个参数。

此函数`add_parameter`用于将一个交互参数（InteractionParameter）对象添加到数据库中。它是`InteractionCRUD`类的一个方法，用于处理与交互参数相关的数据库操作。

函数接收两个参数：
- `db`: 这是一个`Session`对象，代表与数据库的会话，用于执行数据库操作。
- `parameter`: 这是一个`InteractionParameter`对象，包含了要添加到数据库中的参数信息。

函数的执行流程如下：
1. 函数首先尝试调用`InteractionDBInterface.add_parameter`方法，将`parameter`对象添加到数据库中。
2. 如果在添加过程中发生异常，函数会捕获这个异常，并抛出一个`XAgentDBError`错误，其中包含了错误信息。

在项目中的调用情况如下：
在`XAgentServer/application/websockets/base.py`文件中，`add_parameter`函数被用于处理从客户端接收到的数据。当接收到类型为"data"的数据时，会创建一个新的`InteractionParameter`对象，并通过调用`add_parameter`函数将其添加到数据库中。这个过程是在处理WebSocket连接时进行的，确保了每次交互的参数都能被记录下来。

**注意**：
- 在使用`add_parameter`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `parameter`参数应该是一个正确构造的`InteractionParameter`对象，包含了所有必要的参数信息。
- 函数中的异常处理确保了数据库操作的错误能够被适当地捕获和报告，调用者应该准备好处理可能抛出的`XAgentDBError`异常。
## FunctionDef get_parameter
**get_parameter函数**: 该函数的功能是获取特定交互的参数列表。

该函数`get_parameter`是一个类方法，用于从数据库中检索与给定交云ID相关联的参数列表。它接受两个参数：`db`和`interaction_id`。`db`参数是一个数据库会话对象，用于执行数据库操作；`interaction_id`是一个字符串，表示要查询参数的交互的唯一标识符。

函数的执行流程如下：
1. 函数首先尝试调用`InteractionDBInterface.get_parameter`方法，传入`db`和`interaction_id`作为参数，以从数据库中获取参数列表。
2. 如果在执行数据库查询过程中出现任何异常，函数将捕获这个异常，并抛出一个`XAgentDBError`错误，该错误包含了异常的详细信息。

**注意**：
- 使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，且`interaction_id`参数是正确的交互ID。
- 如果数据库查询失败，函数将不会返回参数列表，而是抛出`XAgentDBError`异常，调用者需要妥善处理这个异常。

**输出示例**：
假设数据库中存在与`interaction_id`对应的参数，函数可能返回如下列表：
```python
[
    InteractionParameter(name='param1', value='value1'),
    InteractionParameter(name='param2', value='value2'),
    ...
]
```
如果没有找到对应的参数或发生错误，函数将抛出异常：
```python
XAgentDBError: XAgent DB Error [Interact Module]: <异常信息>
```
## FunctionDef get_init_parameter
**get_init_parameter 函数**: 该函数的功能是获取初始参数。

该函数`get_init_parameter`是一个类方法，用于从数据库中检索与特定交互会话(`interaction_id`)相关联的初始参数。这个函数接受两个参数：`db`和`interaction_id`。`db`参数是一个`Session`对象，代表当前的数据库会话，而`interaction_id`是一个字符串，用于标识特定的交互会话。

函数执行的主要步骤如下：
1. 调用`InteractionDBInterface.get_parameter`方法，传入数据库会话`db`和交互会话标识`interaction_id`，以从数据库中检索相关的参数列表。
2. 从返回的参数列表中获取第一个参数，这个参数被认为是初始参数。
3. 使用获取到的初始参数和交互会话标识`interaction_id`，创建一个`InteractionParameter`对象。这个对象是通过将参数转换为JSON格式并使用`from_json`类方法来创建的。
4. 返回创建的`InteractionParameter`对象。

如果在执行过程中遇到任何异常，函数将捕获这个异常，并抛出一个`XAgentDBError`错误，其中包含了错误信息。这样做可以帮助调用者识别和处理可能发生的数据库相关的错误。

**注意**：
- 在使用这个函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，且`interaction_id`参数正确对应了一个存在的交互会话。
- 如果数据库中没有找到对应的参数，或者出现了其他数据库访问错误，函数将抛出`XAgentDBError`异常。

**输出示例**：
假设数据库中存在交互会话ID为"12345"的初始参数，函数可能返回如下的`InteractionParameter`对象：
```python
InteractionParameter(args={'param1': 'value1', 'param2': 'value2'}, interaction_id='12345', parameter_id=None)
```
在这个示例中，`args`是一个字典，包含了初始参数的键值对，`interaction_id`是交互会话的ID，而`parameter_id`在这个上下文中为`None`。
## FunctionDef search_interaction_by_user_id
**search_interaction_by_user_id 函数**: 该函数的功能是根据用户ID检索用户的交互数据。

该函数`search_interaction_by_user_id`是一个用于从数据库中检索特定用户的交互记录的函数。它接受用户ID作为参数，并支持分页功能，允许调用者指定每页的记录数量（`page_size`）和页码（`page_num`）。

详细代码分析如下：
- `db`: 这是一个`Session`类型的参数，代表当前的数据库会话，用于执行数据库查询。
- `user_id`: 这是一个字符串类型的参数，代表要查询交互记录的用户的唯一标识符。
- `page_size`: 这是一个整型参数，默认值为10，代表每页显示的记录数。
- `page_num`: 这是一个整型参数，默认值为1，代表要查询的页码。

函数内部调用了`InteractionDBInterface`类的`search_interaction_by_user_id`方法，传入相应的参数执行查询，并将查询结果以列表形式返回。每个列表项是一个字典，包含了交互记录的详细信息。

在项目中的调用情况：
在`XAgentServer/application/routers/conv.py`文件中，`search_interaction_by_user_id`函数被用于处理HTTP请求，以获取特定用户的所有交互记录。该路由处理函数`get_all_interactions`使用了依赖注入来获取数据库会话和用户ID，并通过表单接收`page_size`和`page_num`参数。调用`search_interaction_by_user_id`函数后，将结果封装在`ResponseBody`对象中返回给客户端。

**注意**：
- 调用此函数时，确保传入有效的数据库会话对象。
- 用户ID必须是存在于数据库中的有效ID。
- 分页参数`page_size`和`page_num`应根据实际需要进行调整，以适应不同的分页需求。

**输出示例**：
```python
[
    {
        "interaction_id": "123456",
        "user_id": "user123",
        "timestamp": "2021-07-21T12:34:56.789Z",
        "action": "query",
        "details": "用户查询了天气信息"
    },
    {
        "interaction_id": "123457",
        "user_id": "user123",
        "timestamp": "2021-07-21T12:35:56.789Z",
        "action": "response",
        "details": "系统返回了天气信息"
    }
    # ... 更多交互记录 ...
]
```
在这个示例中，返回的是一个包含两个交互记录的列表，每个记录都是一个包含交互详情的字典。
## FunctionDef is_exist
**is_exist函数**: 此函数的功能是检查特定的交互是否存在于数据库中。

此函数`is_exist`属于某个类（在代码中用`cls`表示，但具体类名未给出），用于查询数据库中是否存在具有特定`interaction_id`的交互记录。它接收两个参数：`db`和`interaction_id`。`db`参数代表数据库会话，用于执行数据库查询。`interaction_id`参数是一个字符串，表示要查询的交互的唯一标识符。

函数的执行流程如下：
1. 调用`InteractionDBInterface.is_exist`方法，传入`db`和`interaction_id`作为参数。
2. 如果查询成功，`InteractionDBInterface.is_exist`方法将返回一个布尔值，表示交互是否存在。
3. 如果在查询过程中发生异常，函数将捕获这个异常，并抛出一个`XAgentDBError`错误，其中包含了错误信息。

需要注意的是，此函数中的异常处理非常重要，因为它确保了数据库操作的稳健性。如果数据库查询失败，而不是简单地返回`False`，函数通过抛出`XAgentDBError`提供了更多的错误上下文信息，这对于调试和错误追踪是非常有帮助的。

**注意**：
- 使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话实例。
- `interaction_id`应该是一个有效的交互标识符，否则可能会导致查询不到结果或抛出异常。
- 在调用此函数时，应当做好异常处理，以便在发生数据库错误时能够妥善处理。

**输出示例**：
假设数据库中存在`interaction_id`为"12345"的交互记录，调用`is_exist(db, "12345")`将返回`True`。如果不存在该记录，将返回`False`。如果查询过程中出现异常，将抛出`XAgentDBError`异常。
## FunctionDef update_interaction
**update_interaction函数**：此函数用于更新交互。

该函数接受两个参数：db和base_data。db参数是数据库会话对象，base_data参数是包含基本数据的字典。

函数内部通过调用InteractionDBInterface.update_interaction函数来更新交互。如果更新过程中出现异常，将抛出XAgentDBError异常，并将异常信息包装成字符串后抛出。

**注意**：在使用此函数时，需要确保传入正确的数据库会话对象和正确的基本数据。

此函数在以下文件中被调用：
- 文件路径：XAgentServer/application/routers/conv.py
  调用部分代码如下：
  ```
  InteractionCRUD.update_interaction(db=db, base_data=update_data)
  ```
- 文件路径：XAgentServer/application/websockets/base.py
  调用部分代码如下：
  ```
  InteractionCRUD.update_interaction(db=self.db, base_data={
      "interaction_id": self.client_id,
      "status": "connected",
      "message": "connected",
      "current_step": "0",
      "description": description}
  )
  ```

**注意**：在调用update_interaction函数时，需要确保传入正确的数据库会话对象和正确的基本数据。
## FunctionDef update_interaction_status
**update_interaction_status函数**：此函数的功能是更新交互状态。

此函数用于更新交互的状态信息，包括交互ID、状态、消息和当前步骤。它接受以下参数：

- db：数据库会话对象。
- interaction_id：交互ID。
- status：状态。
- message：消息。
- current_step：当前步骤。

在函数内部，它调用InteractionDBInterface.update_interaction_status函数来更新交互的状态信息。如果更新失败，会抛出XAgentDBError异常。

在项目中，此函数被以下文件调用：

- XAgentServer/application/websockets/base.py：在dispatch函数中调用了此函数，用于更新交互的状态信息。
- XAgentServer/application/websockets/recorder.py：在task_handler函数中调用了此函数，用于更新交互的状态信息。
- XAgentServer/interaction.py：在insert_data函数和ask_for_human_help函数中调用了此函数，用于更新交互的状态信息。

**注意**：在使用此函数时，需要确保传入正确的参数，并处理可能抛出的异常。
## FunctionDef update_interaction_parameter
**update_interaction_parameter 函数**: 该函数的功能是更新交互参数。

该函数`update_interaction_parameter`是用于更新数据库中特定交互的参数。它接受一个数据库会话对象、一个交互ID以及一个交互参数对象作为输入参数，并尝试更新对应的交互参数。

详细代码分析如下：

- `cls`：这是一个类方法的第一个参数，代表类本身。在这个函数中，它没有被直接使用，但是这表明`update_interaction_parameter`函数可能设计为类的一个方法。
- `db: Session`：这个参数是一个数据库会话对象，它是SQLAlchemy的Session实例，用于数据库操作的事务管理。
- `interaction_id: str`：这个参数是一个字符串，代表要更新参数的交互的唯一标识符。
- `parameter: InteractionParameter`：这个参数是一个`InteractionParameter`类型的对象，包含了要更新的参数信息。

函数内部逻辑如下：

1. 函数首先尝试调用`InteractionDBInterface.update_interaction_parameter`方法，传入数据库会话、交互ID和参数对象，以执行更新操作。
2. 如果在更新过程中发生异常，函数会捕获这个异常，并抛出一个`XAgentDBError`错误，该错误包含了异常的相关信息。这样做可以提供更明确的错误信息，并且允许调用者处理特定类型的错误。

**注意**：

- 使用这个函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，且该会话已经正确配置。
- `interaction_id`应该是数据库中已存在的交互的ID，否则更新操作可能会失败。
- `parameter`对象应该符合`InteractionParameter`类的结构，包含了所有必要的参数信息。
- 在调用这个函数时，应该处理可能抛出的`XAgentDBError`异常，以便于在出现数据库错误时，能够采取相应的错误处理措施。
## FunctionDef is_running
**is_running函数**：该函数的功能是检查用户是否正在运行交互。

该函数接受两个参数：db和user_id。db是数据库会话对象，user_id是用户ID。

函数会调用InteractionDBInterface.is_running方法来检查用户是否正在运行交互。如果用户正在运行交互，则返回True；否则返回False。

如果在执行过程中出现异常，函数会抛出XAgentDBError异常，并将异常信息包装成XAgent DB Error [Interact Module]的形式。

该函数在以下文件中被调用：
- XAgentServer/application/websockets/base.py的on_connect方法中
- XAgentServer/application/websockets/recorder.py的on_connect方法中
- XAgentServer/application/websockets/replayer.py的on_connect方法中
- XAgentServer/application/websockets/share.py的on_connect方法中

**注意**：在使用该函数时需要注意以下几点：
- 需要传入有效的数据库会话对象和用户ID参数
- 如果用户正在运行交互，则会抛出XAgentWebSocketConnectError异常

**输出示例**：假设用户正在运行交互，函数的返回值为True。
## FunctionDef delete_interaction
**delete_interaction 函数**: 该函数的功能是删除一个交互记录。

该`delete_interaction`函数是一个用于从数据库中删除特定交互记录的函数。它定义在`XAgentServer/application/cruds/interaction.py`文件中，并且是`InteractionCRUD`类的一个方法。该方法接受两个参数：`db`和`interaction_id`。`db`参数是一个`Session`对象，它代表了当前的数据库会话。`interaction_id`参数是一个字符串，代表了要删除的交互记录的唯一标识符。

函数的执行流程如下：
1. 函数首先尝试调用`InteractionDBInterface.delete_interaction`方法，传入数据库会话和交互记录ID来执行删除操作。
2. 如果在删除过程中发生任何异常，函数会捕获这个异常并抛出一个`XAgentDBError`错误，同时将原始异常信息作为上下文附加在错误信息中。

在项目中，`delete_interaction`函数被`XAgentServer/application/routers/conv.py`文件中的路由处理函数调用。在这个路由处理函数中，它通过依赖注入的方式获取了用户ID、交互记录ID和数据库会话对象。然后，它调用`delete_interaction`函数来删除指定的交互记录，并返回一个包含操作结果的`ResponseBody`对象。

**注意**：
- 在使用`delete_interaction`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，以便能够正确地与数据库进行交互。
- 传入的`interaction_id`应该是一个有效的交互记录ID，否则可能会导致删除失败或删除错误的记录。
- 调用该函数时应该处理可能抛出的`XAgentDBError`异常，以便在发生数据库错误时能够进行适当的错误处理和用户反馈。
## FunctionDef get_shared_interaction
**get_shared_interaction函数**: 此函数的功能是获取共享的交互记录。

该函数`get_shared_interaction`是用于从数据库中检索特定的共享交互记录。它接受两个参数：`db`和`interaction_id`。`db`参数是一个数据库会话对象，用于执行数据库操作；`interaction_id`是一个字符串，表示要检索的交互记录的唯一标识符。

函数首先尝试调用`InteractionDBInterface.get_shared_interaction`方法来获取交互记录。如果找到了对应的记录，该方法将返回一个`InteractionBase`对象；如果没有找到，将返回`None`。

如果在尝试检索记录的过程中发生任何异常，函数将捕获这个异常，并抛出一个`XAgentDBError`错误，其中包含了错误信息。这样做可以帮助调用者识别和处理可能发生的数据库相关的问题。

在项目中，`get_shared_interaction`函数被调用于`XAgentServer/application/routers/conv.py`文件中的`community`函数。在这个上下文中，该函数用于检查一个交互记录是否已经被共享。如果已经存在一个共享的记录，将抛出一个`XAgentWebError`错误，表示交互记录已存在，不需要再次共享。

**注意**：
- 在使用`get_shared_interaction`函数时，确保传入的`db`参数是一个有效的数据库会话对象。
- `interaction_id`应该是一个有效的交互记录标识符，否则可能会导致检索不到记录。
- 调用此函数时应当处理可能抛出的`XAgentDBError`异常。

**输出示例**：
如果函数成功找到了交互记录，它可能返回如下的`InteractionBase`对象实例：
```python
InteractionBase(
    interaction_id="12345",
    user_name="张三",
    create_time="2021-01-01T12:00:00",
    update_time="2021-01-01T12:00:00",
    description="这是一个示例交互记录",
    agent="XAgent",
    mode="模式A",
    is_deleted=False,
    star=0,
    is_audit=False
)
```
如果没有找到对应的记录，函数将返回`None`。
## FunctionDef search_many_shared
**search_many_shared 函数**: 此函数的功能是查询多个共享的交互记录。

此函数`search_many_shared`位于`InteractionCRUD`类中，用于从数据库中检索多个共享的交互记录。它接收三个参数：`db`，`page_size`和`page_index`。`db`参数是一个数据库会话实例，用于执行数据库操作。`page_size`参数用于指定每页显示的记录数量，默认值为20。`page_index`参数用于指定当前的页码，默认值为1。

函数内部，它调用了`InteractionDBInterface.search_many_shared`方法，并传递相应的参数。如果在执行数据库查询过程中出现任何异常，函数将捕获这个异常，并抛出一个`XAgentDBError`错误，其中包含了错误信息。

在项目中，`search_many_shared`函数被`XAgentServer/application/routers/conv.py`文件中的`get_share_interactions`异步路由处理函数调用。在这个上下文中，它用于处理HTTP请求，获取用户共享的所有交互记录。此路由处理函数接收用户ID、页大小和页码作为参数，并依赖于`get_db`函数来提供数据库会话。调用`search_many_shared`函数后，返回的数据被封装在`ResponseBody`中，并返回给客户端。

**注意**：
- 在调用此函数时，确保传递的`db`参数是一个有效的数据库会话实例。
- `page_size`和`page_index`参数应根据实际需要进行设置，以控制分页行为。
- 当数据库操作失败时，需要处理`XAgentDBError`异常，以便于错误追踪和用户友好的错误提示。

**输出示例**：
```python
[
    {
        "id": "123",
        "title": "共享交互记录1",
        "content": "这是一个共享的交互记录示例",
        "shared_by": "用户A",
        "shared_with": ["用户B", "用户C"],
        "created_at": "2023-01-01T12:00:00",
        "updated_at": "2023-01-02T12:00:00"
    },
    {
        "id": "456",
        "title": "共享交互记录2",
        "content": "这是另一个共享的交互记录示例",
        "shared_by": "用户D",
        "shared_with": ["用户E", "用户F"],
        "created_at": "2023-01-03T12:00:00",
        "updated_at": "2023-01-04T12:00:00"
    }
    # ... 更多交互记录 ...
]
```
在这个示例中，返回的是一个列表，列表中的每个元素都是一个包含交互记录详细信息的字典。
## FunctionDef insert_raw
**insert_raw函数**：此函数的功能是将原始数据插入数据库。

该函数接受两个参数，分别是db和process。其中，db是数据库会话对象，process是XAgentRaw类型的原始数据。

函数内部通过调用InteractionDBInterface的insert_raw方法将原始数据插入数据库。如果插入过程中出现异常，会抛出XAgentDBError异常。

在项目中，该函数被以下文件调用：
- XAgentServer/application/routers/conv.py：在community函数中调用了InteractionCRUD.insert_raw方法。
- XAgentServer/application/websockets/base.py：在on_receive函数中调用了InteractionCRUD.insert_raw方法。
- XAgentServer/application/websockets/recorder.py：在task_handler函数中调用了InteractionCRUD.insert_raw方法。
- XAgentServer/interaction.py：在insert_data函数和ask_for_human_help函数中调用了InteractionCRUD.insert_raw方法。

**注意**：在使用该函数时需要注意捕获XAgentDBError异常，并进行相应的处理。
## FunctionDef search_many_raws
**search_many_raws函数**: 该函数的功能是根据交互ID查询多个原始交互数据。

该函数`search_many_raws`是一个类方法，用于从数据库中检索与给定交互ID关联的所有原始交互数据。它接受两个参数：`db`和`interaction_id`。`db`参数是一个数据库会话对象，用于执行数据库查询。`interaction_id`参数是一个字符串，表示要查询的交互的唯一标识符。

函数的主体是一个try-except块。在try块中，它调用`InteractionDBInterface.search_many_raws`方法来执行数据库查询，该查询返回与指定交互ID相关联的所有原始数据记录。然后，它使用列表推导式将每个原始数据记录转换为`XAgentRaw`对象的实例。如果查询成功，它将返回一个`XAgentRaw`对象列表。

如果在查询过程中发生异常，函数将捕获异常并抛出一个`XAgentDBError`错误，其中包含了错误信息。这样做是为了提供更具体的错误上下文，并且允许调用者处理这个特定类型的错误。

在项目中，`search_many_raws`函数被多个地方调用，例如在`conv.py`、`replayer.py`和`share.py`文件中。这些调用场景通常涉及到处理用户交互记录的共享、回放和数据发送功能。

例如，在`conv.py`文件中，该函数被用于在用户尝试共享交互记录时，获取所有相关的原始数据。在`replayer.py`和`share.py`文件中，它被用于在WebSocket连接中向客户端发送交互数据。

**注意**：
- 在使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `interaction_id`应该是一个有效的交互ID，否则查询将返回空结果或抛出异常。
- 调用此函数时应当处理可能抛出的`XAgentDBError`异常。

**输出示例**：
如果函数成功执行，其返回值可能如下所示：
```python
[
    XAgentRaw(...),  # 第一个XAgentRaw对象
    XAgentRaw(...),  # 第二个XAgentRaw对象
    ...
]
```
如果没有找到与给定交互ID匹配的原始数据，或者发生了异常，函数可能返回`None`或抛出`XAgentDBError`异常。
## FunctionDef get_raw
**get_raw函数**: 此函数的功能是从数据库中获取与特定交互ID和节点ID相关联的原始XAgent数据。

此函数`get_raw`是`InteractionCRUD`类的一个方法，它接受三个参数：`db`、`interaction_id`和`node_id`。`db`参数是一个`Session`对象，用于与数据库进行会话。`interaction_id`是一个字符串，表示特定交互的唯一标识符。`node_id`也是一个字符串，表示交互中特定节点的唯一标识符。

函数的主要作用是查询数据库，尝试获取与给定的`interaction_id`和`node_id`相匹配的`XAgentRaw`对象。如果查询成功，返回该对象；如果没有找到匹配的对象，则返回`None`。

在函数的实现中，首先尝试调用`InteractionDBInterface.get_raw`方法执行数据库查询。如果在查询过程中发生异常，函数会捕获这个异常，并抛出一个自定义的`XAgentDBError`错误，其中包含了错误信息。

在项目中，`get_raw`函数被以下文件调用：

1. 文件路径：XAgentServer/application/routers/conv.py
   此处的调用场景是在处理社区分享功能时，需要获取特定交互和节点的原始数据。如果原始数据不存在，则会创建一个新的`XAgentRaw`对象并插入数据库。

2. 文件路径：XAgentServer/interaction.py
   在这里，`get_raw`函数用于在交互过程中获取人类用户的输入数据。如果当前步骤的原始数据存在，并且标记为需要人类输入且已接收，那么函数将返回这些数据。

**注意**：
- 使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `interaction_id`和`node_id`应该是有效的字符串，它们对应数据库中已存在的交互和节点。
- 在调用此函数时，应当处理可能抛出的`XAgentDBError`异常。

**输出示例**：
假设数据库中存在与`interaction_id`和`node_id`相匹配的`XAgentRaw`对象，函数可能返回如下对象：

```python
XAgentRaw(
    interaction_id="123456",
    node_id="node_789",
    data="原始数据内容",
    status="finished",
    is_human=True,
    is_receive=False,
    human_data="用户输入内容"
)
```

如果没有找到匹配的对象，则函数返回`None`。
## FunctionDef get_next_send
**get_next_send函数**: 该函数的功能是获取下一条待发送的交互数据。

该函数`get_next_send`是一个类方法，用于从数据库中检索与特定交互ID关联的下一批未发送的交互数据。它接受两个参数：`db`和`interaction_id`。`db`参数是一个数据库会话对象，用于执行数据库操作；`interaction_id`参数是一个字符串，表示特定的交互ID。

函数内部，它尝试调用`InteractionDBInterface.get_next_send`方法，传入相应的参数来获取数据。如果操作成功，它将返回一个包含`Raw`对象的列表，这些`Raw`对象代表待发送的交互数据。如果在执行过程中遇到任何异常，函数将捕获异常并抛出一个`XAgentDBError`错误，该错误包含了错误信息。

在项目中，`get_next_send`函数被`XAgentServer/application/websockets/base.py`和`XAgentServer/application/websockets/recorder.py`两个文件中的`send_data`方法调用。在这些调用中，函数用于检索待发送的数据，并将其逆序排列，然后逐条检查并发送给客户端。发送数据后，会更新数据库中的发送标志，并在发送失败或完成后停止发送进程。

**注意**:
- 使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `interaction_id`参数必须是一个有效的交互ID，否则可能无法检索到任何数据。
- 函数可能抛出`XAgentDBError`异常，调用者需要妥善处理这一异常。

**输出示例**:
假设数据库中存在与交互ID关联的未发送数据，函数可能返回如下格式的列表：
```python
[
    Raw(node_id="node123", data={"key": "value"}, is_send=False, status="PENDING", ...),
    Raw(node_id="node456", data={"key2": "value2"}, is_send=False, status="PENDING", ...),
    ...
]
```
如果没有待发送的数据或发生异常，函数可能返回`None`或抛出`XAgentDBError`异常。
## FunctionDef update_send_flag
**update_send_flag 函数**: 该函数的功能是更新发送标志。

该`update_send_flag`函数是一个类方法，用于更新数据库中与交互相关的节点的发送标志。这个函数接收三个参数：`db`，`interaction_id`和`node_id`。其中，`db`是数据库会话对象，用于执行数据库操作；`interaction_id`是交互的唯一标识符，表示特定的交互实例；`node_id`是交互中节点的唯一标识符，表示需要更新发送状态的特定节点。

函数内部，它尝试调用`InteractionDBInterface.update_send_flag`方法来实际更新数据库中的发送标志。如果在更新过程中发生任何异常，函数会捕获这个异常并抛出一个`XAgentDBError`错误，其中包含了错误的详细信息。

在项目中，`update_send_flag`函数被以下文件中的代码调用：

1. 文件路径：XAgentServer/application/websockets/base.py
   在`send_data`异步方法中，该函数被用来标记一条交互数据已经被发送给客户端。在发送数据给客户端之后，通过调用`update_send_flag`函数来更新数据库中对应节点的发送状态，以避免重复发送相同的数据。

2. 文件路径：XAgentServer/application/websockets/recorder.py
   在`send_data`异步方法中，该函数同样被用来更新发送状态。在这里，它用于记录交互数据是否已经成功发送给客户端，并在发送完成后更新数据库中的状态。

**注意**：
- 在使用`update_send_flag`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，以便能够正确执行数据库操作。
- `interaction_id`和`node_id`参数应该是有效的标识符，它们对应数据库中已存在的交互实例和节点。
- 在调用此函数时，应当处理可能抛出的`XAgentDBError`异常，确保异常情况下能够正确响应并记录错误信息。
- 由于该函数涉及数据库操作，应确保在事务安全的上下文中调用，避免数据不一致的问题发生。
## FunctionDef update_receive_flag
**update_receive_flag 函数**: 该函数的功能是更新接收标志。

该函数`update_receive_flag`是一个类方法，用于更新数据库中特定交互节点的接收标志。它接收三个参数：`db`、`interaction_id`和`node_id`。其中，`db`是一个数据库会话对象，用于执行数据库操作；`interaction_id`是交互的唯一标识符；`node_id`是节点的唯一标识符。

函数内部，首先尝试调用`InteractionDBInterface.update_receive_flag`方法，将`db`、`interaction_id`和`node_id`作为参数传递给该方法。这个调用实际上是执行更新操作，将特定交互节点的接收标志设置为新的值。

如果在尝试更新数据库时发生异常，函数会捕获这个异常，并抛出一个`XAgentDBError`错误。这个错误包含了一个错误消息，该消息包括了"XAgent DB Error [Interact Module]"的前缀，以及异常的字符串表示，这有助于调试和识别问题所在。

**注意**：
- 使用这个函数时，需要确保传入的`db`对象是一个有效的数据库会话实例。
- `interaction_id`和`node_id`应该是有效的标识符，它们对应数据库中已存在的交互和节点。
- 在调用此函数时，应当处理可能抛出的`XAgentDBError`异常，以便于在出现数据库错误时能够妥善处理。
- 该函数不返回任何值，它的主要作用是影响数据库中的数据状态。
## FunctionDef update_human_data
**update_human_data 函数**: 此函数的功能是更新人类交互数据。

update_human_data 函数是 XAgentServer 项目中用于更新与特定交互节点相关的人类交互数据的方法。它接受一个数据库会话对象、交互ID、节点ID和一个包含人类数据的字典作为参数。

详细代码分析如下：
- `db`: 这是一个 SQLAlchemy Session 对象，用于执行数据库操作。
- `interaction_id`: 这是一个字符串，代表特定的交互实例的唯一标识符。
- `node_id`: 这是一个字符串，代表交互中特定节点的唯一标识符。
- `human_data`: 这是一个字典，包含了与节点相关的人类交互数据。

函数内部，首先尝试调用 `InteractionDBInterface.update_human_data` 方法来更新数据库中的人类数据。如果在更新过程中出现任何异常，函数将捕获这个异常并抛出一个 `XAgentDBError` 错误，其中包含了错误信息。

在项目中的调用情况：
在 `XAgentServer/application/websockets/base.py` 文件中，`update_human_data` 函数被用于处理从客户端接收到的数据。当 WebSocket 接收到客户端发送的类型为 "data" 的数据时，会从数据中提取出 `node_id` 和 `args`（人类数据），然后调用 `update_human_data` 函数来更新数据库中对应节点的人类交互数据。

**注意**：
- 在使用 `update_human_data` 函数时，确保提供的 `db` 参数是有效的数据库会话对象。
- `interaction_id` 和 `node_id` 必须是有效的字符串，且在数据库中有对应的记录。
- `human_data` 应该是一个字典，包含了需要更新的数据。
- 调用此函数时需要处理可能抛出的 `XAgentDBError` 异常，以确保程序的健壮性。
## FunctionDef insert_error
**insert_error函数**: 该函数的功能是向数据库中插入错误信息。

该函数`insert_error`是在`XAgentServer`项目中用于处理交互错误的一个重要组成部分。当交互过程中出现错误时，此函数会被调用以记录错误信息。

详细代码分析如下：

- 函数接受四个参数：`db`（数据库会话对象），`interaction_id`（交互ID），`message`（错误信息），以及`status`（状态，默认为"failed"）。
- 函数首先尝试创建一个`XAgentRaw`对象，该对象包含了错误信息和相关的状态。
- `XAgentRaw`对象的`node_id`字段是通过`uuid.uuid4().hex`生成的唯一标识符。
- `create_time`和`update_time`字段使用当前时间的字符串格式进行赋值。
- `InteractionDBInterface.insert_raw`方法被用来将`XAgentRaw`对象插入到数据库中。
- 如果在插入过程中发生异常，函数会捕获异常并抛出一个`XAgentDBError`错误，其中包含了错误信息。

在项目中的调用情况：

1. 在`XAgentServer/application/websockets/base.py`文件中，`task_handler`方法中，如果在处理交互任务时出现`XAgentError`异常，会调用`insert_error`函数记录错误信息，并更新交互状态为"failed"。

2. 在`XAgentServer/application/websockets/recorder.py`文件中，`task_handler`方法中，同样在处理交互任务时如果捕获到`XAgentError`异常，也会调用`insert_error`函数来记录错误信息，并更新交互状态为"failed"。

**注意**：在使用`insert_error`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，以及`interaction_id`确实对应一个正在进行的交互过程。此外，错误信息`message`应当清晰地描述错误的内容，以便于后续的错误分析和处理。

**输出示例**：此函数的返回值是一个`XAgentRaw`对象，该对象包含了错误记录的详细信息。例如：

```python
XAgentRaw(
    node_id="a1b2c3d4e5f6g7h8i9j0",
    interaction_id="1234567890",
    current="",
    step=0,
    data="出现未知错误",
    file_list=[],
    status="failed",
    do_interrupt=False,
    wait_seconds=0,
    ask_for_human_help=False,
    create_time="2023-04-01 12:00:00",
    update_time="2023-04-01 12:00:00",
    is_deleted=False,
    is_human=False,
    human_data={},
    human_file_list=[],
    is_send=False,
    is_receive=False,
    include_pictures=False,
)
```

在这个示例中，`node_id`是一个随机生成的唯一标识符，`interaction_id`是交互的ID，`data`字段包含了错误信息，`status`字段表示当前的状态为"failed"。
## FunctionDef add_share
**add_share函数**: 此函数的功能是向数据库中添加共享交互记录。

add_share函数接受两个参数：一个数据库会话对象`db`和一个共享交互对象`share`。该函数的主要作用是将用户的交互信息添加到数据库中，以便其他用户可以访问和查看这些共享的交互信息。

具体来说，函数首先尝试调用`InteractionDBInterface.add_share`方法，将`share`对象添加到数据库中。如果在添加过程中发生任何异常，函数将捕获这个异常，并抛出一个自定义的`XAgentDBError`错误，其中包含了错误信息，以便调用者能够了解到底发生了什么问题。

在项目中，`add_share`函数被`XAgentServer/application/routers/conv.py`文件中的`community`函数调用。在`community`函数中，首先检查是否已经存在相同的共享交互记录，如果存在，则抛出`XAgentWebError`错误。接着，检查交互记录中是否包含状态为完成（FINISHED）的节点，如果没有，则同样抛出`XAgentWebError`错误。之后，创建交互记录的存储目录，并将上传的文件保存到该目录中。最后，创建一个新的共享交互记录对象`share`，并调用`add_share`函数将其添加到数据库中。

**注意**：
- 在使用`add_share`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，以便能够正确执行数据库操作。
- 传入的`share`对象应该包含所有必要的共享交互信息，如交互ID、用户名、创建时间、更新时间、描述、代理、模式等。
- 在调用此函数之前，应该进行必要的错误处理和数据校验，确保不会因为重复添加或数据不完整而导致错误。
- 如果在添加共享交互记录的过程中发生异常，调用者需要处理抛出的`XAgentDBError`，以便采取适当的错误处理措施。
## FunctionDef get_finish_status
**get_finish_status函数**: 该函数的作用是获取交互是否完成的状态。

该函数`get_finish_status`接收两个参数：`db`和`interaction_id`。`db`是一个数据库会话对象，用于执行数据库操作；`interaction_id`是一个字符串，代表需要查询的交互ID。

函数的主要逻辑是调用`InteractionDBInterface.get_finish_status`方法，传入数据库会话和交互ID，以获取该交互是否已完成。如果交互完成，函数返回`True`；否则返回`False`。

在实际调用中，如`XAgentServer/application/routers/conv.py`文件中的`share_interaction`函数，首先检索特定的交互记录，然后调用`get_finish_status`函数来判断该交互是否已经完成。如果交互未完成，则返回错误信息，告知用户交互尚未结束。

如果在查询过程中发生异常，函数会捕获异常并抛出`XAgentDBError`错误，其中包含了错误信息，这有助于调用者识别和处理可能出现的数据库相关问题。

**注意**：
- 在使用`get_finish_status`函数时，确保传入的`db`参数是一个有效的数据库会话对象，且`interaction_id`参数是一个有效的交互ID。
- 函数内部可能抛出的`XAgentDBError`异常应当被妥善处理，以避免程序崩溃或不可预期的行为。

**输出示例**：
假设有一个交互ID为"12345"的交互已经完成，调用`get_finish_status(db, "12345")`将返回`True`。如果该交互未完成或不存在，将返回`False`。如果数据库操作出现异常，将抛出`XAgentDBError`异常。
***
