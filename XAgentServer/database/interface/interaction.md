# ClassDef InteractionDBInterface
**InteractionDBInterface函数**：这个类的功能是与交互数据库进行交互的接口。

该类包含以下函数：

- `search_many_interaction(db: Session) -> list[InteractionBase]`：搜索多个交互记录。
- `get_interaction(db: Session, interaction_id: str) -> InteractionBase | None`：根据交互ID获取交互记录。
- `create_interaction(db: Session, base: InteractionBase) -> None`：创建交互记录。
- `get_ready_interaction(db: Session, user_id: str) -> InteractionBase | None`：根据用户ID获取准备就绪的交互记录。
- `add_parameter(db: Session, parameter: InteractionParameter) -> None`：为交互记录添加参数。
- `get_parameter(db: Session, interaction_id: str) -> list`：获取交互记录的参数列表。
- `search_interaction_by_user_id(db: Session, user_id: str, page_size: int = 20, page_num: int = 1) -> list[dict]`：根据用户ID搜索交互记录。
- `is_exist(db: Session, interaction_id: str) -> bool`：检查交互记录是否存在。
- `update_interaction(db: Session, base_data: dict) -> None`：更新交互记录。
- `update_interaction_status(db: Session, interaction_id: str, status: str, message: str, current_step: int) -> None`：更新交互记录的状态。
- `update_interaction_parameter(db: Session, interaction_id: str, parameter: InteractionParameter) -> None`：更新交互记录的参数。
- `is_running(db: Session, user_id: str) -> bool`：检查用户是否正在运行交互记录。
- `delete_interaction(db: Session, interaction_id: str) -> None`：删除交互记录。
- `get_shared_interaction(db: Session, interaction_id: str) -> SharedInteractionBase | None`：根据交互ID获取共享交互记录。
- `search_many_shared(db: Session, page_size: int = 20, page_index: int = 1) -> list[dict]`：搜索多个共享交互记录。
- `insert_raw(db: Session, process: XAgentRaw) -> None`：插入交互过程记录。
- `search_many_raws(db: Session, interaction_id: str) -> list[XAgentRaw]`：搜索多个交互过程记录。
- `get_raw(db: Session, interaction_id: str, node_id: str) -> XAgentRaw | None`：根据交互ID和节点ID获取交互过程记录。
- `get_next_send(db: Session, interaction_id: str) -> List[Raw] | None`：获取下一个待发送的交互过程记录。
- `update_send_flag(db: Session, interaction_id: str, node_id: str) -> None`：更新交互过程记录的发送标志。
- `update_receive_flag(db: Session, interaction_id: str, node_id: str) -> None`：更新交互过程记录的接收标志。
- `update_human_data(db: Session, interaction_id: str, node_id: str, human_data: dict) -> None`：更新交互过程记录的人工数据。
- `insert_error(db: Session, interaction_id: str, message: str) -> XAgentRaw`：插入错误信息记录。
- `add_share(db: Session, shared: SharedInteractionBase) -> None`：添加共享交互记录。
- `get_finish_status(db: Session, interaction_id: str) -> bool`：获取交互记录的完成状态。

**注意**：使用该类时需要注意以下事项：

- 需要传入有效的数据库会话对象（db: Session）。
- 部分函数可能会抛出XAgentDBError异常，需要进行异常处理。

**输出示例**：模拟代码返回值的可能外观。

请注意：
- 生成的内容中不应包含Markdown的标题和分隔符语法。
- 主要使用目标语言进行编写。如果需要，可以在分析和描述中使用一些英文单词，以增强文档的可读性，因为不需要将函数名或变量名翻译成目标语言。
## FunctionDef search_many_interaction
**search_many_interaction函数**: 该函数的功能是查询数据库中的多个交互记录。

该函数`search_many_interaction`定义在`XAgentServer/database/interface/interaction.py`文件中，是一个类方法，用于从数据库中检索所有的交互记录。它接受一个数据库会话对象`db`作为参数，该参数是SQLAlchemy的`Session`类型，用于执行数据库查询。

函数的主体部分只有两行代码。首先，它使用`db.query(Interaction).all()`查询语句从`Interaction`表中检索所有记录。`Interaction`是一个模型类，代表数据库中的交互记录表。`.all()`方法将返回一个包含所有交互记录的列表。

接下来，函数通过列表推导式将每个数据库中的交互记录转换为`InteractionBase`类的实例。`InteractionBase.from_db(interaction)`是一个工厂方法，它接受一个交互记录对象，并将其转换为`InteractionBase`对象。这样做的目的是将数据库模型对象转换为更通用的基础类对象，可能是为了后续处理的方便或者是为了与数据库解耦。

在项目中，`search_many_interaction`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名方法调用。在这个上下文中，它被用于处理与交互记录相关的业务逻辑。如果在检索交互记录的过程中发生异常，会捕获该异常并抛出一个自定义的`XAgentDBError`错误，这样可以提供更清晰的错误信息，并且可以从异常中恢复。

**注意**：
- 在使用`search_many_interaction`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- 该函数返回的是`InteractionBase`对象的列表，因此调用者需要了解`InteractionBase`类的结构和属性。
- 在异常处理中，异常被重新抛出时使用了`from e`语法，这样可以保留原始异常的堆栈信息，便于调试。

**输出示例**：
```python
[
    InteractionBase(id=1, user_id=123, ...),
    InteractionBase(id=2, user_id=456, ...),
    ...
]
```
以上是一个模拟的返回值示例，其中`InteractionBase`对象包含了交互记录的各种属性，例如`id`和`user_id`等。实际的属性取决于`InteractionBase`类的定义和数据库中交互记录的结构。
## FunctionDef get_interaction
**get_interaction函数**: 此函数的功能是根据交互ID获取交互信息。

`get_interaction`函数定义在`XAgentServer/database/interface/interaction.py`文件中，其主要作用是从数据库中查询并返回特定ID的交互信息。如果找到对应的交互记录，它会将记录转换为`InteractionBase`对象；如果没有找到，它会返回`None`。

详细代码分析如下：
- 函数接收两个参数：`db`和`interaction_id`。其中，`db`是数据库会话对象，类型为`Session`；`interaction_id`是一个字符串，表示要查询的交互ID。
- 在函数内部，首先使用`db.query(Interaction)`构造一个查询对象，这个查询对象会在`Interaction`表中查找。
- 接着，通过`filter`方法添加过滤条件，即`Interaction.interaction_id == interaction_id`和`Interaction.is_deleted.is_not(True)`。这两个条件分别表示查询的交互ID必须匹配，且该交互记录没有被标记为删除。
- 使用`first()`方法尝试获取第一个匹配的记录。如果数据库中存在符合条件的记录，`first()`会返回该记录；如果不存在，会返回`None`。
- 如果查询到了交互记录，函数会使用`InteractionBase.from_db(interaction)`将数据库模型对象转换为`InteractionBase`对象。如果没有查询到记录，则返回`None`。

在项目中，`get_interaction`函数被调用于`XAgentServer/application/cruds/interaction.py`文件中。在这个文件中，`get_interaction`函数被封装在一个类方法中，并且添加了异常处理。如果在查询过程中发生异常，会抛出`XAgentDBError`异常，并附带错误信息。

**注意**：
- 使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- 在调用此函数时，应当处理可能出现的异常，例如数据库查询错误。

**输出示例**：
假设数据库中存在一个交互ID为`"12345"`的记录，调用`get_interaction(db, "12345")`可能会返回如下的`InteractionBase`对象示例：
```python
InteractionBase(
    interaction_id="12345",
    user_id="user_001",
    content="用户与系统的交互内容",
    timestamp=datetime.datetime(2023, 4, 1, 12, 0),
    # ... 其他字段
)
```
如果交互ID不存在，则函数会返回`None`。
## FunctionDef get_ready_interaction
**get_ready_interaction函数**：此函数的功能是根据用户ID获取数据库中状态为“ready”的交互对象。

此函数`get_ready_interaction`是一个类方法，它接受两个参数：`db`和`user_id`。`db`参数是一个数据库会话对象，用于执行数据库查询。`user_id`参数是一个字符串，表示用户的唯一标识符。

函数的主要逻辑如下：
1. 使用`db.query(Interaction)`开始一个查询，其中`Interaction`是一个映射到数据库中交互表的模型类。
2. 通过`filter`方法添加查询条件，筛选出用户ID等于`user_id`且状态为`ready`的交互对象。
3. 使用`first()`方法从查询结果中获取第一个对象，如果没有符合条件的对象，则返回`None`。
4. 如果查询到了交互对象，那么使用`InteractionBase.from_db(interaction)`将其转换为`InteractionBase`对象，否则返回`None`。

在项目中，`get_ready_interaction`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名方法调用。调用此函数时，如果数据库操作出现异常，会捕获异常并抛出一个`XAgentDBError`错误，其中包含了错误信息。

**注意**：
- 在使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `user_id`参数应该是一个有效的用户ID字符串。
- 调用此函数时应当处理可能出现的异常，例如在调用处使用`try...except`语句。

**输出示例**：
假设数据库中存在一个用户ID为`"user123"`且状态为`"ready"`的交互对象，那么调用`get_ready_interaction(db, "user123")`可能会返回一个`InteractionBase`对象。如果没有找到符合条件的交互对象，则返回`None`。
## FunctionDef create_interaction
**create_interaction函数**: 该函数的功能是创建交互记录。

该`create_interaction`函数是一个数据库操作函数，用于在数据库中创建一个新的交互记录。它接收两个参数：`db`和`base`。`db`参数是一个数据库会话对象，用于执行数据库操作；`base`参数是一个`InteractionBase`类型的对象，它包含了要创建的交互记录的基本信息。

函数的执行流程如下：
1. 函数首先将`base`对象转换为字典格式，这是通过调用`base.to_dict()`方法实现的。
2. 然后，使用`db.add()`方法将转换后的字典作为`Interaction`模型的实例添加到数据库会话中。
3. 接着，调用`db.commit()`方法提交会话，以保存对数据库所做的更改。
4. 最后，函数返回`None`，表示创建操作完成。

在项目中，`create_interaction`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名方法调用。在调用过程中，如果数据库操作出现异常，会捕获该异常并抛出一个自定义的`XAgentDBError`错误，以便于错误处理和日志记录。

**注意**：
- 在使用`create_interaction`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，且`base`参数是一个正确初始化的`InteractionBase`对象。
- 调用该函数时，应当在一个异常处理结构中进行，以便于捕获和处理可能发生的数据库错误。
- 函数不返回创建的交互记录对象，而是返回`None`。如果需要获取创建的记录，需要在调用函数后，使用其他查询方法来检索。

**输出示例**：
由于`create_interaction`函数返回值为`None`，因此不会有输出示例。函数的主要作用是对数据库进行操作，而不是返回数据。
## FunctionDef add_parameter
**add_parameter 函数**: 该函数的功能是为交互添加参数。

该函数`add_parameter`是定义在`XAgentServer/database/interface/interaction.py`文件中的一个方法，它的主要作用是将一个交互参数对象添加到数据库中。这个函数接受两个参数：`db`和`parameter`。`db`参数是一个`Session`对象，它代表了当前的数据库会话，用于执行数据库操作。`parameter`参数是一个`InteractionParameter`类型的对象，它包含了要添加到数据库的参数信息。

函数的执行流程如下：
1. 函数首先通过调用`parameter.to_dict()`方法将`InteractionParameter`对象转换为一个字典。
2. 然后，使用`db.add()`方法将这个字典转换成的参数对象添加到数据库会话中。
3. 接着，调用`db.commit()`方法提交会话，确保参数被保存到数据库中。
4. 最后，函数返回`None`，表示添加参数的操作没有返回值。

在项目中，`add_parameter`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名方法调用。在这个调用场景中，`add_parameter`方法被用于添加一个交互参数，并处理可能出现的异常。如果在添加参数的过程中发生异常，它会抛出一个`XAgentDBError`错误，并附带错误信息。

**注意**：
- 在使用`add_parameter`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，并且`parameter`参数是一个正确初始化的`InteractionParameter`对象。
- 函数在操作数据库时可能会抛出异常，调用者需要妥善处理这些异常，例如在`XAgentServer/application/cruds/interaction.py`文件中所示，使用`try-except`语句捕获并处理异常。

**输出示例**：
由于`add_parameter`函数没有返回值，它执行成功后不会有输出。函数的正常执行不会产生直接的输出结果，但会影响数据库中的数据状态。如果函数执行成功，数据库中将新增一条与`parameter`对象相对应的参数记录。如果执行失败，将抛出异常，不会有任何数据被添加到数据库中。
## FunctionDef search_interaction_by_user_id
**search_interaction_by_user_id函数**: 该函数的功能是根据用户ID搜索用户的交互记录。

该函数`search_interaction_by_user_id`是一个用于根据用户ID检索用户交互记录的方法。它接受数据库会话、用户ID、页大小和页码作为参数，并返回一个包含交互记录列表的字典。

详细代码分析如下：
- 函数接收四个参数：`db`（数据库会话对象），`user_id`（用户ID字符串），`page_size`（可选，每页显示的记录数，默认为20），`page_num`（可选，页码，默认为1）。
- 首先，函数使用`db.query`和`func.count`来查询与给定用户ID关联的、未被删除的交互记录的总数。
- 然后，函数查询数据库以获取特定用户的交互记录列表。这里使用了过滤条件，只选择状态为`FINISHED`的、未被删除的记录。
- 查询结果根据页码和页大小进行了分页处理，即通过`limit`和`offset`方法实现。
- 对于查询到的每条交互记录，函数使用`InteractionBase.from_db(interaction).to_dict`方法将其转换为字典格式，并排除了一些不需要的字段。
- 函数还调用了`cls.get_parameter`方法来获取每条交互记录的参数，并将其添加到结果字典中。
- 最后，函数返回一个包含总记录数和当前页交互记录列表的字典。

**注意**：
- 使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `user_id`参数应为有效的用户ID字符串。
- `page_size`和`page_num`参数可以根据需要调整，以控制分页的大小和页码。
- 函数返回的字典中，`total`键对应的值为总记录数，`rows`键对应的值为当前页的交互记录列表。

**输出示例**：
```python
{
    "total": 100,  # 总记录数
    "rows": [  # 当前页的交互记录列表
        {
            "interaction_id": "123456",
            "parameters": [
                {"param1": "value1", "param2": "value2"}
            ],
            # ... 其他交互记录字段
        },
        # ... 更多交互记录
    ]
}
```

在项目中的调用情况：
在`XAgentServer/application/cruds/interaction.py`文件中，有一个同名的方法`search_interaction_by_user_id`，它实际上是调用了`InteractionDBInterface`类中的`search_interaction_by_user_id`方法。在这个调用中，`page_size`的默认值被设置为10，而不是在原始函数中的20。这表明在不同的上下文中，函数的分页大小可以根据实际需要进行调整。
## FunctionDef search_many_shared
**search_many_shared函数**: 该函数的功能是从社区中搜索多个共享的交互记录。

该函数`search_many_shared`定义在数据库接口层，用于查询社区共享的交互记录。它接受数据库会话、页面大小和页面索引作为参数，并返回一个包含总记录数和当前页交互记录列表的字典。

详细代码分析如下：
- 函数接收三个参数：`db`（数据库会话），`page_size`（页面大小，默认值为20），`page_index`（页面索引，默认值为1）。
- 首先，使用`db.query`和`func.count`来计算未被删除且已审核的共享交互记录的总数。
- 接着，查询相应的共享交互记录列表，按照点赞数降序排列，并根据页面大小和页面索引进行分页。
- 遍历查询结果，将每条记录转换为字典格式，并排除不需要的字段（如`record_dir`和`is_deleted`）。
- 对于每条交互记录，通过调用`cls.get_parameter`方法获取其参数，并添加到记录的字典中。
- 最后，函数返回一个字典，包含总记录数`total`和当前页的交互记录列表`rows`。

**注意**：
- 在调用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- 分页参数`page_size`和`page_index`应根据实际需要进行调整，以适应不同的分页需求。
- 函数依赖于`SharedInteraction`和`SharedInteractionBase`模型，确保这些模型在数据库中正确定义且与函数逻辑相匹配。

**输出示例**：
调用`search_many_shared`函数可能返回的结果示例如下：
```json
{
  "total": 100,
  "rows": [
    {
      "interaction_id": 1,
      "title": "示例交互记录1",
      "description": "这是一个示例描述",
      "parameters": [
        {
          "name": "参数1",
          "value": "值1"
        },
        {
          "name": "参数2",
          "value": "值2"
        }
      ],
      "star": 10
    },
    {
      "interaction_id": 2,
      "title": "示例交互记录2",
      "description": "这是另一个示例描述",
      "parameters": [
        {
          "name": "参数A",
          "value": "值A"
        }
      ],
      "star": 5
    }
    // ...更多交互记录
  ]
}
```
在这个示例中，`total`表示共享交互记录的总数，`rows`是一个列表，包含了每条交互记录的详细信息，如交互记录ID、标题、描述、参数列表和点赞数等。
## FunctionDef get_shared_interaction
**get_shared_interaction函数**: 此函数的功能是根据交互ID获取共享交互信息。

此函数`get_shared_interaction`定义在`XAgentServer/database/interface/interaction.py`文件中，其主要作用是通过特定的交互ID从数据库中查询并返回一个共享交互对象。如果找到相应的共享交互对象，它将返回一个`SharedInteractionBase`实例；如果没有找到，则返回`None`。

详细代码分析如下：

- 函数接收两个参数：`db`和`interaction_id`。`db`是一个数据库会话对象，类型为`Session`，用于执行数据库查询操作；`interaction_id`是一个字符串，表示要查询的共享交互的唯一标识符。
- 函数内部首先使用`db.query(SharedInteraction)`构造一个查询对象，该查询对象用于查询`SharedInteraction`表。
- 接下来，通过`filter`方法添加查询条件，确保`interaction_id`字段与传入的`interaction_id`相匹配，并且`is_deleted`字段为`False`，以排除已被标记为删除的记录。
- 使用`first()`方法尝试获取查询结果的第一条记录，如果没有找到匹配的记录，`first()`将返回`None`。
- 如果查询到了共享交互对象，函数将使用`SharedInteractionBase.from_db(interaction)`将数据库模型对象转换为`SharedInteractionBase`实例；如果没有查询到，则直接返回`None`。

**注意**：
- 在调用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，且`interaction_id`参数是正确的交互ID。
- 此函数可能返回`None`，调用者应当检查返回值以确定是否成功获取到了共享交互对象。
- 在异常处理方面，调用此函数的代码应当准备好捕获并处理可能发生的数据库错误。

**输出示例**：
如果查询成功，可能的返回值示例为一个`SharedInteractionBase`实例，包含共享交互的相关信息。如果没有找到对应的共享交互，返回值为`None`。

在项目中的调用情况分析：
此函数在`XAgentServer/application/cruds/interaction.py`文件中被调用。在这里，`get_shared_interaction`函数被用于获取共享交互信息，并且包含了异常处理逻辑。如果在查询过程中发生异常，将抛出一个`XAgentDBError`异常，以便调用者可以捕获并处理数据库相关的错误。
## FunctionDef is_exist
**is_exist函数**: 此函数的功能是检查数据库中是否存在指定的交互记录。

is_exist函数是一个数据库查询函数，用于判断特定的交互记录（Interaction）是否存在于数据库中。该函数接收两个参数：一个数据库会话对象（db）和一个交互记录的ID（interaction_id）。函数返回一个布尔值，表示指定的交互记录是否存在且未被标记为删除。

详细代码分析如下：
- 参数`db`是一个`Session`类型的对象，代表当前的数据库会话，用于执行数据库查询。
- 参数`interaction_id`是一个字符串，代表要查询的交互记录的唯一标识符。
- 函数内部首先使用`db.query(Interaction)`构造一个查询对象，该查询对象用于从`Interaction`表中检索数据。
- 接着，通过`filter`方法添加查询条件，确保只查找`interaction_id`字段与传入的`interaction_id`相匹配且`is_deleted`字段为`False`的记录。
- `first()`方法用于获取查询结果中的第一条记录，如果没有符合条件的记录，则返回`None`。
- 最后，函数通过判断查询结果是否为`None`来返回布尔值，如果查询结果不为`None`，则表示交互记录存在，返回`True`；否则返回`False`。

在项目中的调用情况：
is_exist函数在`XAgentServer/application/cruds/interaction.py`文件中被调用。在调用时，尝试使用`InteractionDBInterface.is_exist`方法来检查交互记录是否存在。如果在查询过程中发生异常，则会捕获异常并抛出`XAgentDBError`错误，以便调用者能够处理可能出现的数据库错误。

**注意**：
- 在使用is_exist函数时，确保传入的`db`参数是一个有效的数据库会话对象。
- 传入的`interaction_id`应该是一个有效的交互记录ID，且为字符串类型。
- 调用此函数时应当处理可能抛出的异常，特别是在数据库操作中，异常处理是必不可少的。

**输出示例**：
假设数据库中存在一个交互记录ID为"12345"的记录，并且该记录未被删除，那么调用`is_exist(db, "12345")`将返回`True`。如果该记录不存在或已被删除，则返回`False`。
## FunctionDef update_interaction
**update_interaction 函数**: 该函数的功能是更新交互记录。

该函数`update_interaction`是用于更新数据库中的交互记录的。它接收两个参数：`db`和`base_data`。`db`是一个数据库会话对象，它允许函数执行数据库操作。`base_data`是一个字典，包含了要更新的交互数据。

函数首先检查`base_data`中是否包含关键字`interaction_id`，如果不存在，则抛出`XAgentError`异常，提示需要提供交互记录的ID。接下来，函数通过`interaction_id`在数据库中查询相应的交互记录。如果记录不存在，同样会抛出`XAgentError`异常，提示交互记录不存在。

如果交互记录存在，函数将遍历`base_data`字典中的键值对，并使用`setattr`函数更新交互记录对象的属性。这里跳过了`interaction_id`键，因为它是用于查找记录的，而不需要更新。在更新了所有其他属性后，函数设置交互记录的`update_time`为当前时间，并格式化为`"%Y-%m-%d %H:%M:%S"`字符串。最后，通过`db.commit()`提交更改到数据库。

在项目中，`update_interaction`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名函数调用。调用函数封装了异常处理，如果在更新交互记录的过程中发生任何异常，它会捕获异常并抛出`XAgentDBError`，提供了更具体的错误信息。

**注意**：
- 在调用`update_interaction`函数之前，确保传入的`db`参数是一个有效的数据库会话对象。
- `base_data`字典必须包含`interaction_id`键，否则函数将无法执行。
- 传入的`base_data`字典中的键应该与数据库模型`Interaction`的属性相匹配，以确保正确更新记录。
- 在实际应用中，应当处理好异常情况，确保数据库的完整性和一致性。
- 函数中使用了`datetime.now().strftime("%Y-%m-%d %H:%M:%S")`来获取格式化的当前时间字符串，这依赖于`datetime`模块，因此在使用前需要确保已经导入了该模块。
## FunctionDef update_interaction_status
**update_interaction_status 函数**: 该函数的功能是更新交互状态。

该函数`update_interaction_status`是一个用于更新数据库中特定交互记录状态的方法。它接收五个参数：数据库会话对象`db`，交互ID`interaction_id`，新的状态`status`，相关消息`message`，以及当前步骤`current_step`。

详细的参数说明如下：
- `db (Session)`：这是一个数据库会话对象，用于执行数据库操作。
- `interaction_id (str)`：这是一个字符串，代表需要更新状态的交互的唯一标识符。
- `status (str)`：这是一个字符串，表示交互的新状态。
- `message (str)`：这是一个字符串，包含与状态更新相关的消息或说明。
- `current_step (int)`：这是一个整数，表示交互进行到的当前步骤。

函数首先会通过`interaction_id`查询数据库中是否存在对应的交互记录。如果记录不存在，会抛出`XAgentError`异常，提示交互不存在。

如果交互记录存在，函数会更新该记录的状态、消息、当前步骤以及更新时间。更新时间会被设置为当前的时间戳。

最后，函数会提交数据库事务以保存更改。

在项目中，`update_interaction_status`函数被调用于`XAgentServer/application/cruds/interaction.py`文件中。在这个文件中，函数被封装在一个类方法中，并且有异常处理的逻辑。如果在更新交互状态的过程中发生任何异常，会捕获这个异常并抛出`XAgentDBError`异常，提供了错误处理的机制。

**注意**：
- 在调用此函数时，确保传入的`db`参数是一个有效的数据库会话对象。
- 传入的`interaction_id`应该是数据库中已存在的交互记录的ID。
- 在实际应用中，应当处理好可能出现的异常，例如在调用处捕获`XAgentError`和`XAgentDBError`，以便于错误追踪和用户提示。
- 更新操作会影响数据库中的数据，因此在调用此函数时应确保操作的合理性和必要性。
## FunctionDef update_interaction_parameter
**update_interaction_parameter函数**: 该函数的功能是更新交互参数。

此函数`update_interaction_parameter`用于更新数据库中与特定交云互动相关联的参数。它接受一个数据库会话对象、一个交互ID以及一个交互参数对象，并尝试在数据库中找到与给定交互ID和参数ID相匹配的参数记录。如果找到了相应的参数记录，则会用传入的参数对象更新该记录；如果没有找到，则会在数据库中添加一个新的参数记录。

详细代码分析如下：

- `db (Session)`: 这是一个数据库会话对象，用于执行数据库操作。
- `interaction_id (str)`: 这是一个字符串，表示需要更新参数的交互ID。
- `parameter (InteractionParameter)`: 这是一个`InteractionParameter`对象，包含了需要更新的参数信息。

函数首先使用`db.query(Parameter)`构建一个查询，通过`filter`方法筛选出与给定的`interaction_id`和`parameter.parameter_id`匹配的参数记录。然后，使用`first()`方法尝试获取查询结果的第一条记录。

如果`db_parameter`为`None`，即数据库中不存在匹配的参数记录，则使用`db.add(Parameter(**parameter.to_dict()))`将传入的参数对象转换为字典，并添加到数据库会话中。最后，调用`db.commit()`提交事务，确保更改被保存到数据库中。

在项目中调用此函数的情况如下：

文件路径：XAgentServer/application/cruds/interaction.py
在这个文件中，`update_interaction_parameter`函数被封装在一个类方法中，并在尝试更新数据库时捕获异常。如果发生异常，将抛出一个`XAgentDBError`错误，提供了错误处理的机制。

**注意**：
- 在使用此函数时，需要确保传入的`db`对象是一个有效的数据库会话实例。
- `interaction_id`应该是一个存在于数据库中的有效交互ID。
- `parameter`对象应该包含有效的参数数据，并且其`to_dict`方法能够正确地将参数对象转换为字典格式。
- 在调用此函数时，需要处理可能抛出的异常，确保程序的健壮性。
## FunctionDef is_running
**is_running函数**: 该函数的功能是检查用户是否只运行了一个交互。

该`is_running`函数定义在`XAgentServer/database/interface/interaction.py`文件中，属于数据库接口层的一部分。它的主要作用是查询数据库，检查指定用户ID的用户当前是否有正在运行或等待状态的交互。这个功能对于确保用户不会同时运行多个交互过程是非常重要的。

函数接受两个参数：
- `db (Session)`: 这是一个数据库会话对象，用于执行数据库查询。
- `user_id (str)`: 这是一个字符串，表示用户的唯一标识符。

函数的返回值是一个布尔值：
- 如果找到至少一个用户的交互处于“running”或“waiting”状态，函数返回`True`。
- 如果没有找到任何符合条件的交互，函数返回`False`。

具体实现中，函数使用了SQLAlchemy ORM的查询接口。它构造了一个查询，该查询在`Interaction`表中筛选出与给定`user_id`相匹配且状态为“running”或“waiting”的记录，并调用`.first()`方法尝试获取第一个匹配的记录。如果查询结果不为空（即至少有一个匹配的记录），则表示用户有正在运行的交互，函数返回`True`；否则返回`False`。

在项目中，`is_running`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名方法调用。在这个上下文中，它被用于在CRUD操作中检查用户是否有正在运行的交互。如果在执行查询过程中发生异常，会抛出一个`XAgentDBError`异常，以便调用者可以处理数据库错误。

**注意**：
- 在使用`is_running`函数时，需要确保传入正确的数据库会话对象和用户ID。
- 异常处理是调用此函数时需要注意的，确保在数据库查询出现问题时能够妥善处理异常。

**输出示例**：
假设数据库中存在用户ID为"12345"的用户，并且该用户有一个状态为"running"的交互，那么调用`is_running(db, "12345")`将返回`True`。如果该用户没有正在运行或等待的交互，调用将返回`False`。
## FunctionDef get_parameter
**get_parameter函数**: 此函数的功能是获取交互运行参数。

该`get_parameter`函数定义在`XAgentServer/database/interface/interaction.py`文件中，它的主要作用是从数据库中检索与特定交互ID相关联的参数列表。这些参数是由用户在交互过程中输入的数据。

详细代码分析如下：
- 函数接收两个参数：`db`和`interaction_id`。
  - `db`参数是一个数据库会话对象，用于执行数据库查询。
  - `interaction_id`参数是一个字符串，代表特定的交互ID。
- 函数内部，使用`db.query()`方法来查询`Raw`表。
- 查询条件包括：
  - `Raw.interaction_id`等于传入的`interaction_id`。
  - `Raw.is_human`为`True`，表示这些数据是由人类用户输入的。
  - `Raw.human_data`不为`None`，确保只检索有实际数据的记录。
- 查询结果按照`Raw.step`字段升序排序，并使用`all()`方法获取所有匹配的记录。
- 函数返回一个列表，包含所有匹配记录的`human_data`字段。

在项目中，`get_parameter`函数被其他模块调用，以获取交互过程中的参数数据。例如，在`XAgentServer/application/cruds/interaction.py`文件中，有两个地方调用了这个函数：
1. `get_parameter`方法：直接调用`InteractionDBInterface.get_parameter`来获取参数列表，并处理异常，如果发生异常则抛出`XAgentDBError`。
2. `get_init_parameter`方法：调用`InteractionDBInterface.get_parameter`来获取参数列表，然后取出列表中的第一个参数作为初始化参数，并构造成`InteractionParameter`对象返回。

此外，在`XAgentServer/database/interface/interaction.py`文件中，`get_parameter`函数也被用于在搜索交互时附加参数信息。在`search_interaction_by_user_id`和`search_many_shared`两个方法中，都调用了`get_parameter`函数来获取每个交互的参数，并将其添加到返回的数据中。

**注意**：
- 在调用此函数时，需要确保传入的`db`是一个有效的数据库会话对象。
- 传入的`interaction_id`应该是一个存在于数据库中的有效ID。
- 函数返回的是一个列表，列表中的每个元素都是与交互相关的参数数据。

**输出示例**：
假设数据库中有两条与特定`interaction_id`相关的记录，且它们的`human_data`分别为`{"name": "Alice"}`和`{"age": 30}`，那么函数的返回值可能如下所示：
```python
[{"name": "Alice"}, {"age": 30}]
```
## FunctionDef delete_interaction
**delete_interaction函数**: 该函数的功能是删除一个交互记录。

该函数`delete_interaction`定义在`XAgentServer/database/interface/interaction.py`文件中，其主要作用是在数据库中将指定的交互记录标记为已删除。函数接收两个参数：`db`和`interaction_id`。`db`是一个数据库会话对象，用于执行数据库操作；`interaction_id`是一个字符串，表示要删除的交互记录的唯一标识符。

函数首先通过`interaction_id`在数据库中查询对应的交互记录。如果查询结果为空，即数据库中不存在该交互记录，函数将抛出一个`XAgentError`异常，异常信息为"interaction is not exist"，表示交互记录不存在。

如果查询到了对应的交互记录，函数将该记录的`is_deleted`字段标记为`True`，表示该记录已被删除。随后，调用`db.commit()`提交数据库事务，以确保更改被保存。

在项目中，`delete_interaction`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名函数调用。调用时，尝试执行删除操作，并捕获可能发生的任何异常。如果捕获到异常，则抛出一个`XAgentDBError`异常，异常信息中包含了原始异常信息，以便于问题追踪和调试。

**注意**：
- 在调用`delete_interaction`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，且`interaction_id`确实对应数据库中的一个存在的交互记录。
- 函数中使用了异常处理来确保数据库操作的健壮性，调用者应当准备好处理可能抛出的`XAgentError`和`XAgentDBError`异常。
- 该函数仅标记交互记录为已删除，而不是从数据库中完全移除该记录，这样做可以保留记录的历史数据，以便于未来的数据恢复或分析。
## FunctionDef add_share
**add_share函数**: 该函数的功能是添加共享交互。

add_share函数定义在XAgentServer项目的数据库接口模块中，具体位于`XAgentServer/database/interface/interaction.py`文件。此函数的主要作用是将社区中共享的交互信息添加到数据库中。它接受两个参数：一个数据库会话对象`db`和一个`SharedInteractionBase`实例`shared`。`SharedInteractionBase`是一个基础类，代表了共享交互的数据结构。

函数的具体实现很简单，它首先通过`shared.to_dict()`方法将`SharedInteractionBase`实例转换为字典形式，然后创建一个`SharedInteraction`实例，并将其添加到数据库会话中。这里的`SharedInteraction`是一个数据库模型，代表数据库中的共享交互表。

在项目中，`add_share`函数被`XAgentServer/application/cruds/interaction.py`文件中的`add_share`方法调用。调用时，传入的参数包括一个数据库会话对象和一个共享对象。如果在添加共享交互到数据库的过程中发生异常，调用方会捕获这个异常，并抛出一个`XAgentDBError`错误，这是一个自定义的异常，用于表示数据库操作相关的错误。

**注意**：
- 在使用`add_share`函数时，需要确保传入的数据库会话对象`db`是有效的，并且已经正确配置了数据库连接。
- `shared`参数必须是一个`SharedInteractionBase`的实例，且其数据结构应当符合共享交互的要求。
- 调用此函数时应当处理可能抛出的异常，确保程序的健壮性。
- 在实际的生产环境中，可能需要对共享的交互内容进行验证和清洗，以防止恶意数据的注入。
## FunctionDef insert_raw
**insert_raw 函数**: 该函数的功能是插入一个交互过程用于记录。

该函数`insert_raw`是一个数据库交互函数，用于将一个交互过程（`XAgentRaw`实例）插入到数据库中。它主要用于记录XAgent系统中的交互过程的原始数据。

详细代码分析如下：

- 函数接受两个参数：`db`和`process`。`db`是一个数据库会话对象，用于执行数据库操作；`process`是一个`XAgentRaw`类型的实例，代表要插入的交互过程。
- 函数首先查询数据库中是否存在与`process.interaction_id`相同的`Interaction`对象。如果不存在，则抛出`XAgentError`异常，表示交互不存在。
- 如果存在相应的`Interaction`对象，函数会查询是否已有相同`interaction_id`的`Raw`记录，并且这些记录没有被标记为删除。它会选择步骤值最大的记录作为`exist_process`。
- 如果`exist_process`存在，说明已有交互过程记录，函数会将`process.step`设置为`exist_process.step + 1`，以确保步骤值是递增的。
- 如果`exist_process`不存在，说明是第一条交互过程记录，函数会将`process.step`设置为0。
- 接下来，函数会将`process`对象转换为字典，并添加到数据库会话中，然后提交会话以保存记录。

在项目中调用情况分析：

- 在`XAgentServer/application/cruds/interaction.py`文件中，`insert_raw`函数被封装在`InteractionDBInterface`类中，并被调用以插入原始交互数据。
- 在`insert_raw`的调用代码中，如果数据库操作发生异常，会捕获异常并抛出`XAgentDBError`，提供了错误处理机制，确保了数据库操作的健壮性。
- 另一个调用点是`insert_error`函数，它使用`insert_raw`来插入错误信息。这表明`insert_raw`函数不仅用于正常的交互过程记录，也用于错误处理。

**注意**：
- 使用`insert_raw`函数时，需要确保传入的`process`对象是有效的`XAgentRaw`实例，并且其`interaction_id`属性已经设置。
- 在调用此函数之前，应该确保数据库会话`db`是活跃的，并且在操作完成后正确处理会话（如提交或回滚）。
- 异常处理是必要的，因为在插入过程中可能会遇到数据库约束冲突或其他问题，应当在调用函数的地方捕获并处理这些异常。
## FunctionDef search_many_raws
**search_many_raws函数**: 该函数的功能是查询与特定交互ID关联的所有未删除的原始处理记录，并按步骤顺序返回。

该函数`search_many_raws`定义在`XAgentServer/database/interface/interaction.py`文件中，它接收两个参数：`db`和`interaction_id`。`db`参数是数据库会话对象，用于执行数据库查询；`interaction_id`参数是一个字符串，表示要查询的交互ID。

函数的主体是一个查询表达式，它使用`db.query(Raw)`从数据库中查询`Raw`表。查询条件是`Raw.interaction_id`等于传入的`interaction_id`，并且`Raw.is_deleted`字段为`False`，表示只查询未被删除的记录。查询结果按`Raw.step`字段升序排序，使用`.all()`方法获取所有满足条件的记录。

返回值是一个列表，包含了查询到的`XAgentRaw`对象，这些对象代表了与特定交互ID关联的处理过程列表。

在项目中，`search_many_raws`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名方法调用。调用时，它尝试从`InteractionDBInterface.search_many_raws`获取原始处理记录列表，并将每个记录转换为`XAgentRaw`对象。如果在查询过程中出现异常，会抛出`XAgentDBError`异常。

**注意**：
- 在使用`search_many_raws`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- 传入的`interaction_id`应该是一个有效的交互ID，否则可能查询不到任何记录。
- 函数假设`Raw`表中有`interaction_id`和`is_deleted`字段，以及`step`字段用于排序。
- 调用者需要处理可能抛出的`XAgentDBError`异常。

**输出示例**：
```python
[
    XAgentRaw(id='1', interaction_id='abc123', step=1, is_deleted=False, ...),
    XAgentRaw(id='2', interaction_id='abc123', step=2, is_deleted=False, ...),
    ...
]
```
以上示例展示了可能的返回值，其中包含了多个`XAgentRaw`对象，每个对象都包含了与特定交互ID关联的处理步骤信息。
## FunctionDef get_raw
**get_raw函数**: 该函数的功能是根据交互ID和节点ID获取原始数据。

该函数`get_raw`定义在`XAgentServer/database/interface/interaction.py`文件中，它的作用是从数据库中查询并返回与特定交互ID和节点ID相匹配的原始数据记录。该函数接受三个参数：`cls`表示调用该函数的类本身，`db`是一个`Session`对象，用于数据库会话，`interaction_id`和`node_id`分别是交互ID和节点ID的字符串。

函数内部，通过`db.query(Raw)`创建一个查询对象，然后使用`filter`方法添加查询条件。这些条件包括：`Raw.interaction_id`等于传入的`interaction_id`，`Raw.node_id`等于传入的`node_id`，以及`Raw.is_deleted.is_(False)`确保查询的数据未被标记为删除。最后，使用`first()`方法尝试获取第一个匹配的记录。

如果查询成功找到匹配的记录，函数将返回这个记录对象；如果没有找到，将返回`None`。

在项目中，`get_raw`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名方法调用。调用该函数时，会传入数据库会话对象、交互ID和节点ID，并期望返回一个`XAgentRaw`对象。如果在查询过程中发生异常，会捕获异常并抛出一个`XAgentDBError`错误，其中包含了错误信息。

**注意**：在使用`get_raw`函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，`interaction_id`和`node_id`参数也应该是正确的字符串格式。此外，调用者需要处理可能抛出的`XAgentDBError`异常。

**输出示例**:
假设数据库中存在一个与`interaction_id`和`node_id`匹配的`Raw`记录，函数可能返回如下对象：
```python
<Raw(id='123', interaction_id='abc-123', node_id='node-456', data='...', is_deleted=False)>
```
如果没有找到匹配的记录，则函数返回`None`。
## FunctionDef get_next_send
**get_next_send函数**: 此函数的功能是获取下一个待发送的交互过程。

此函数`get_next_send`位于`XAgentServer/database/interface/interaction.py`文件中，它的主要作用是从数据库中查询并返回与给定交互ID相关联的下一个未发送的交互过程。这个函数是一个类方法，它接收两个参数：`db`和`interaction_id`。`db`参数是一个数据库会话对象，用于执行数据库查询；`interaction_id`参数是一个字符串，表示特定的交互ID。

在函数内部，它使用传入的数据库会话对象`db`来执行一个查询。这个查询是针对`Raw`模型的，它会筛选出所有与给定`interaction_id`相匹配、`is_send`字段为`False`（表示尚未发送）以及`is_deleted`字段为`False`（表示未被删除）的记录。查询结果按照`Raw.step`字段降序排序，并使用`.all()`方法获取所有符合条件的记录。

函数返回的是一个`XAgentRaw`对象列表，这个列表包含了所有符合条件的交互过程。如果没有找到任何记录，它将返回一个空列表。

在项目中，`get_next_send`函数被`XAgentServer/application/cruds/interaction.py`文件中的`get_next_send`方法调用。在这个上下文中，该方法尝试调用`InteractionDBInterface.get_next_send`来获取下一个待发送的交互过程，并在遇到异常时抛出`XAgentDBError`错误。

**注意**：
- 在使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `interaction_id`参数应该是一个有效的交互ID，否则查询将不会返回任何结果。
- 函数假设数据库中的`Raw`模型具有`interaction_id`、`is_send`和`is_deleted`等字段。
- 如果数据库查询失败或其他异常情况发生，调用此函数的方法应该妥善处理异常。

**输出示例**：
假设数据库中有以下`Raw`记录：

| id | interaction_id | is_send | is_deleted | step |
|----|----------------|---------|------------|------|
| 1  | abc123         | False   | False      | 3    |
| 2  | abc123         | True    | False      | 2    |
| 3  | abc123         | False   | False      | 1    |

调用`get_next_send(db, "abc123")`将返回以下列表：

```python
[
    {
        "id": 1,
        "interaction_id": "abc123",
        "is_send": False,
        "is_deleted": False,
        "step": 3
    },
    {
        "id": 3,
        "interaction_id": "abc123",
        "is_send": False,
        "is_deleted": False,
        "step": 1
    }
]
```

这个列表按`step`字段降序排列，只包含`is_send`为`False`和`is_deleted`为`False`的记录。
## FunctionDef update_send_flag
**update_send_flag 函数**: 此函数的功能是更新发送标志

此函数`update_send_flag`的主要作用是在数据库中更新特定交互过程的发送标志。当一个交互过程成功发送后，此函数会将对应的发送标志设为真（True），表示该过程已经被发送，不再需要被发送。

详细代码分析如下：

- 函数接受三个参数：`db`（数据库会话对象），`interaction_id`（交互ID），`node_id`（节点ID）。
- 函数首先通过`interaction_id`和`node_id`在数据库中查询对应的交互过程实例。
- 如果查询结果为空，即数据库中不存在对应的交互过程，函数将抛出`XAgentError`异常，异常信息为"process is not exist"，表示所指定的交互过程不存在。
- 如果查询到了对应的交互过程，函数将该过程的`is_send`属性设置为True，并更新`update_time`为当前时间。
- 最后，函数通过`db.commit()`提交数据库事务，以保存更改。

在项目中的调用情况：

此函数在`XAgentServer/application/cruds/interaction.py`文件中被调用。在该文件中，`update_send_flag`函数被封装在一个类方法中，用于处理可能出现的异常。如果在更新发送标志的过程中发生任何异常，将会捕获这些异常并抛出`XAgentDBError`异常，异常信息会包含"XAgent DB Error [Interact Module]"，以及原始异常信息。

**注意**：

- 在使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `interaction_id`和`node_id`必须是有效的标识符，它们用于定位数据库中的特定交互过程。
- 函数调用者需要处理可能抛出的`XAgentError`异常，确保在交互过程不存在时能够妥善处理。
- 在调用此函数后，应确保处理任何可能的`XAgentDBError`异常，以便在数据库操作中出现问题时能够通知调用者。
## FunctionDef update_receive_flag
**update_receive_flag 函数**: 该函数的功能是更新接收标志。如果接收成功，则更新标志。如果这个标志为 True，意味着该过程已经从人类那里接收到了。

该函数`update_receive_flag`定义在`XAgentServer`项目的数据库接口模块中，其主要作用是在数据库中更新特定交互过程的接收状态。当一个交互过程被人类接收后，该函数会被调用来标记该过程的接收状态为真（True）。

详细代码分析如下：

- 函数接收三个参数：`db`（数据库会话对象），`interaction_id`（交互ID），和`node_id`（节点ID）。
- 函数首先查询数据库中的`Raw`表，查找与给定的`interaction_id`和`node_id`相匹配的记录。
- 如果没有找到对应的过程记录，函数会抛出一个`XAgentError`异常，表示过程不存在。
- 如果找到了对应的记录，函数会将该记录的`is_receive`字段更新为 True，表示该过程已被接收。
- 同时，函数还会更新该记录的`update_time`字段为当前时间，以记录更新的时间点。
- 最后，函数会提交数据库事务以保存更改。

在项目中调用该函数的情况如下：

文件路径为`XAgentServer/application/cruds/interaction.py`，在这个文件中，定义了一个同名的`update_receive_flag`函数，该函数封装了数据库接口层的调用，并处理了可能出现的异常。如果在更新接收标志的过程中发生异常，它会捕获异常并抛出一个`XAgentDBError`异常，以通知调用者数据库操作出现了错误。

**注意**：
- 在使用该函数时，需要确保传入的`db`参数是一个有效的数据库会话对象，`interaction_id`和`node_id`也需要是有效的标识符。
- 函数调用者应当处理可能抛出的`XAgentError`异常，确保程序的健壮性。
- 由于该函数涉及数据库操作，应确保其在适当的事务控制下被调用，以避免数据不一致的问题。
## FunctionDef update_human_data
**update_human_data 函数**: 此函数的功能是更新人类交互数据。

此函数`update_human_data`用于在数据库中更新与特定交互和节点相关的人类数据。它是在处理用户与系统交互时，用户提供的数据需要被记录和更新到数据库中的场景下使用的。

详细代码分析如下：

- 函数接收四个参数：`db`（数据库会话对象），`interaction_id`（交互ID），`node_id`（节点ID）以及`human_data`（人类数据，以字典形式提供）。
- 函数首先查询数据库中的`Raw`表，以查找与给定的`interaction_id`和`node_id`相匹配且未被删除（`is_deleted`为`False`）的记录。
- 如果没有找到对应的记录，函数将抛出`XAgentError`异常，提示“process is not exist”（流程不存在）。
- 如果找到了对应的记录，函数将设置该记录的`is_receive`和`is_human`字段为`True`，表示已接收到数据且数据来源于人类。
- 然后，函数将`human_data`参数中的数据更新到找到的记录的`human_data`字段。
- 最后，函数调用`db.commit()`提交更改到数据库。

在项目中的调用情况：

此函数在`XAgentServer/application/cruds/interaction.py`文件中被调用。在调用时，封装了一个同名的`update_human_data`方法，该方法尝试调用`InteractionDBInterface`类的`update_human_data`函数，并传入相应的参数。如果在更新过程中出现异常，它将捕获异常并抛出`XAgentDBError`异常，提供了错误处理机制。

**注意**：

- 在使用此函数时，需要确保传入的`db`参数是一个有效的数据库会话对象。
- `interaction_id`和`node_id`应该是预先存在于数据库中的有效ID，否则会抛出异常。
- 传入的`human_data`字典应包含所有需要更新的用户数据。
- 在调用此函数之前，应该处理好异常情况，以便在发生错误时能够正确响应。
- 此函数涉及数据库操作，因此应确保在事务安全的上下文中使用，避免数据不一致的问题。
## FunctionDef insert_error
**insert_error 函数**: 该函数的功能是在交互失败时插入错误消息。

该函数`insert_error`用于在数据库中记录一个交互过程中发生的错误。当一个交互（interaction）失败时，这个函数会被调用来插入错误信息，这些信息将会在交互列表中显示。

详细代码分析如下：

- 函数接收三个参数：`db`（数据库会话对象），`interaction_id`（交互ID），以及`message`（错误信息）。
- `db`参数的类型为`Session`，它是一个数据库会话实例，用于执行数据库操作。
- `interaction_id`是一个字符串，代表了特定交互的唯一标识符。
- `message`也是一个字符串，包含了错误的具体信息。

函数内部的处理流程如下：

1. 首先，创建一个`Raw`对象，这个对象代表数据库中的一条记录。
2. `Raw`对象的`node_id`字段使用`uuid.uuid4().hex`生成一个唯一的标识符。
3. `interaction_id`字段设置为传入的交互ID。
4. `current`字段留空，因为当前没有执行的节点。
5. `step`字段设置为0，表示步骤编号。
6. `data`字段设置为传入的错误信息。
7. `file_list`字段是一个空列表，因为这里没有文件列表。
8. `status`字段设置为`StatusEnum.FAILED`，表示这个交互的状态是失败的。
9. `create_time`和`update_time`字段使用当前时间的字符串格式表示创建和更新时间。

接下来，使用`db.add(process)`将这个`Raw`对象添加到数据库会话中，然后调用`db.commit()`提交事务，确保错误信息被保存到数据库中。

**注意**：
- 在使用这个函数之前，需要确保传入的`db`是一个有效的数据库会话实例，并且已经正确配置。
- `interaction_id`需要是一个有效的交互ID，通常在交互开始时就已经生成。
- 错误信息`message`应该提供足够的细节，以便于理解交互失败的原因。
- 函数不返回任何值，也不抛出异常，但在实际使用中，应当处理可能出现的数据库操作异常。
## FunctionDef get_finish_status
**get_finish_status函数**: 此函数的功能是获取交互是否完成的状态。

此函数`get_finish_status`位于`XAgentServer/database/interface/interaction.py`文件中，它的主要作用是查询数据库中某个交互（interaction）是否已经完成。它接受两个参数：数据库会话对象`db`和交互的唯一标识符`interaction_id`。函数通过查询数据库，检查与给定`interaction_id`相匹配的`Raw`记录是否存在，并且该记录的状态是否为"finished"，同时该记录没有被标记为已删除（`is_deleted`字段为`False`）。

具体来说，函数首先使用`db.query(Raw)`构建一个查询对象，然后通过`filter`方法添加过滤条件，这些条件包括：`Raw.interaction_id`字段等于传入的`interaction_id`，`Raw.is_deleted`字段为`False`，以及`Raw.status`字段等于字符串"finished"。接着，使用`first()`方法尝试获取查询结果中的第一条记录。

如果查询到符合条件的记录，说明该交互已经完成，函数返回`True`；如果没有查询到符合条件的记录，说明该交互未完成或不存在，函数返回`False`。

在项目中，`get_finish_status`函数被`XAgentServer/application/cruds/interaction.py`文件中的同名方法调用。在调用时，它尝试获取交互完成状态，并在出现异常时抛出`XAgentDBError`错误，以便于错误处理和异常追踪。

**注意**：
- 确保在调用此函数时，数据库会话对象`db`是有效的，并且已经与数据库建立了连接。
- 传入的`interaction_id`应该是有效的交互ID，否则查询将无法找到对应的记录。
- 函数的异常处理应当在调用此函数的上层逻辑中进行，以确保程序的健壮性。

**输出示例**：
如果交互已完成，函数可能返回：
```python
True
```
如果交互未完成或不存在，函数可能返回：
```python
False
```
***
