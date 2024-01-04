# ClassDef User
**User类功能**：User类用于表示XAgent用户。

User类具有以下属性：
- id：用户ID，主键，整数类型。
- user_id：用户ID，字符串类型，唯一索引。
- email：用户邮箱，字符串类型，唯一索引。
- name：用户名，字符串类型。
- token：用户令牌，字符串类型。
- available：用户是否可用，布尔类型，默认为True。
- is_beta：用户是否为Beta用户，布尔类型，默认为False。
- deleted：用户是否已删除，布尔类型，默认为False。
- corporation：用户所属公司，文本类型。
- industry：用户所属行业，文本类型。
- position：用户职位，字符串类型。
- create_time：用户创建时间，字符串类型。
- update_time：用户更新时间，字符串类型。

**注意**：使用此代码时需要注意以下几点：
- User类是XAgent用户的数据模型，用于存储用户的相关信息。
- id、user_id和email属性具有唯一性约束，需要保证其唯一性。
- 可以通过调用get_user_list方法获取所有用户的列表。
- 可以通过调用get_user方法根据user_id或email获取用户信息。
- 可以通过调用is_exist方法判断用户是否存在。
- 可以通过调用token_is_exist方法判断用户的令牌是否存在。
- 可以通过调用add_user方法添加用户。
- 可以通过调用update_user方法更新用户信息。
- user_is_valid方法用于验证用户的有效性。

以上是User类的详细说明和使用注意事项。
***
# ClassDef Interaction
**Interaction类的功能**: Interaction类用于表示XAgent的交互对象。

Interaction类具有以下属性：
- id：交互对象的唯一标识符。
- interaction_id：交互对象的ID。
- user_id：用户ID。
- create_time：创建时间。
- update_time：更新时间。
- description：描述信息。
- agent：代理。
- mode：模式。
- recorder_root_dir：录制根目录。
- file_list：文件列表。
- status：状态。
- message：消息。
- current_step：当前步骤。
- is_deleted：是否已删除。
- call_method：调用方法。

Interaction类提供以下方法：
- search_many_interaction：搜索多个交互对象。
- get_interaction：根据交互ID获取交互对象。
- get_ready_interaction：根据用户ID获取准备就绪的交互对象。
- create_interaction：创建交互对象。
- search_interaction_by_user_id：根据用户ID搜索交互对象。
- is_exist：检查交互对象是否存在。
- update_interaction：更新交互对象。
- update_interaction_status：更新交互对象的状态。
- is_running：检查用户是否正在运行交互对象。
- delete_interaction：删除交互对象。
- insert_raw：插入交互对象的原始数据。

**注意**：在使用Interaction类时，需要注意以下几点：
- 需要提供有效的交互ID和用户ID。
- 在更新交互对象时，需要提供有效的交互数据。
- 在删除交互对象时，需要确保交互对象存在且未被删除。
- 在插入原始数据时，需要确保交互对象存在且未被删除，并提供有效的原始数据。

以上是Interaction类的功能和使用注意事项的详细说明。
***
# ClassDef Parameter
**Parameter 类的功能**: 该类的功能是定义XAgent交互参数的数据模型。

Parameter 类继承自 Base 类，是一个数据模型类，用于表示XAgent系统中的交互参数。这个类定义了交互参数的数据结构和存储方式。

- `__tablename__` 属性指定了数据库中存储这些参数的表名为 "interaction_parameters"。
- `id` 字段是一个整数类型的主键，用于唯一标识一个参数实例。
- `interaction_id` 字段是一个字符串类型，用于存储与参数相关联的交互ID，它在表中是唯一的并且被索引，以便快速查询。
- `parameter_id` 字段是一个字符串类型，用于存储参数的ID。
- `args` 字段是一个JSON类型，用于存储参数的具体值。

在项目中，Parameter 类被调用于 `XAgentServer/database/interface/interaction.py` 文件中，主要用于以下几个场景：

1. **添加交互参数**：`add_parameter` 方法接收一个 `InteractionParameter` 对象，将其转换为字典，并使用 `Parameter` 类创建一个新的参数实例，然后将其添加到数据库会话中，并提交会话以保存更改。

2. **更新交互参数**：`update_interaction_parameter` 方法通过给定的 `interaction_id` 和 `parameter` 对象来更新特定的交互参数。首先，它会查询数据库中是否存在对应的参数实例，如果不存在，则创建一个新的 `Parameter` 实例并添加到数据库中；如果存在，则更新现有的参数实例。最后，提交数据库会话以保存更改。

**注意**：
- 在使用 Parameter 类时，需要确保 `interaction_id` 和 `parameter_id` 的唯一性，以防止数据冲突。
- 在更新参数时，应该先查询数据库中是否存在对应的参数实例，以决定是添加新实例还是更新现有实例。
- 在操作数据库时，不要忘记提交会话，以确保更改被正确保存。
- `args` 字段存储的是JSON格式的数据，因此在处理这个字段时，需要确保数据格式的正确性和可序列化性。
***
# ClassDef SharedInteraction
**SharedInteraction类的功能**：SharedInteraction类是用于表示社区共享交互的对象。

SharedInteraction类具有以下属性：
- id：交互的唯一标识符，为整数类型。
- interaction_id：交互的ID，为字符串类型。
- user_name：交互的用户名，为字符串类型。
- create_time：交互的创建时间，为字符串类型。
- update_time：交互的更新时间，为字符串类型。
- description：交互的描述信息，为文本类型。
- agent：交互的代理，为字符串类型。
- mode：交互的模式，为字符串类型。
- is_deleted：交互是否已删除，为布尔类型，默认为False。
- star：交互的星级评价，为整数类型，默认为0。
- is_audit：交互是否已审核，为布尔类型，默认为False。

SharedInteraction类的方法包括：
- from_db()：从数据库中获取SharedInteraction对象。
- to_dict()：将SharedInteraction对象转换为字典形式。

**注意**：使用该代码时需要注意以下几点：
- SharedInteraction类用于表示社区共享交互的信息，可以通过该类的属性获取交互的各项信息。
- 可以通过from_db()方法从数据库中获取SharedInteraction对象。
- 可以通过to_dict()方法将SharedInteraction对象转换为字典形式。
***
# ClassDef Raw
**Raw类功能**：Raw类用于表示原始数据。

Raw类继承自Base类，用于映射到数据库中的raw表。该表存储了与交互过程相关的原始数据信息，包括节点ID、交互ID、当前节点、步骤、数据、文件列表、状态等。

**属性**：
- id：主键，表示数据的唯一标识。
- node_id：节点ID，表示数据所属的节点。
- interaction_id：交互ID，表示数据所属的交互过程。
- current：当前节点，表示当前所在的节点。
- step：步骤，表示数据所在的步骤。
- data：数据，存储与交互过程相关的数据。
- file_list：文件列表，存储与交互过程相关的文件列表。
- status：状态，表示数据的状态。
- do_interrupt：是否中断，表示是否中断交互过程。
- wait_seconds：已等待时间，表示已等待的时间。
- ask_for_human_help：是否需要人工干预，表示是否需要人工干预。
- create_time：创建时间，表示数据的创建时间。
- update_time：更新时间，表示数据的更新时间。
- is_deleted：是否删除，表示数据是否被删除。
- is_human：是否人工已经输入，表示是否已经有人工输入的数据。
- human_data：人工输入数据，存储人工输入的数据。
- human_file_list：人工文件列表，存储人工输入的文件列表。
- is_send：是否推送前端，表示是否已经将数据推送给前端。
- is_receive：是否接收前端消息，表示是否已经接收到前端的消息。
- include_pictures：是否包含png，表示数据中是否包含png图片。

**注意**：
- Raw类用于表示原始数据，通过与其他类的关联，可以构建完整的交互过程。
- 数据库中的raw表存储了与交互过程相关的原始数据信息，包括节点ID、交互ID、当前节点、步骤、数据、文件列表、状态等。
- Raw类的属性对应了raw表中的字段，通过这些属性可以对原始数据进行操作和查询。
- Raw类的对象可以通过数据库操作进行创建、更新和删除等操作。
- Raw类的对象可以通过与其他类的关联，获取与交互过程相关的数据和信息。

以上是Raw类的详细说明，该类用于表示原始数据，并提供了与交互过程相关的属性和方法。在使用该类时，需要注意数据的状态和相关的操作。
***
# ClassDef RunningRecord
**RunningRecord类的功能**：RunningRecord类用于表示运行记录。

RunningRecord类具有以下属性：

- id：记录的唯一标识符，为整数类型。
- record_id：记录的记录ID，为字符串类型。
- current：当前节点，为字符串类型。
- node_id：节点ID，为字符串类型。
- node_type：节点类型，为字符串类型，可选值为[now_subtask_id, llm_input_pair, tool_server_pair, query, config]。
- data：代理数据，为JSON类型。
- create_time：创建时间，为字符串类型。
- update_time：更新时间，为字符串类型。
- is_deleted：是否已删除，为布尔类型，默认为False。

该类在以下文件中被调用：

文件路径：XAgentServer/database/interface/recorder.py
调用部分代码如下：
```python
def get_record_list(cls, db: Session, record_id: str) -> list[XAgentRunningRecord]:
    """获取所有记录

    Args:
        db (Session): 数据库会话
        record_id (str): 记录ID

    Returns:
        list[XAgentRunningRecord]: 记录列表
    """
    records = db.query(RunningRecord).filter(
        RunningRecord.record_id == record_id).all()
    return [XAgentRunningRecord.from_db(Recorder) for Recorder in records]
```

该类还在其他文件中被调用，具体调用代码如上所示。

**注意**：使用该类时需要注意以下几点：
- 需要导入`RunningRecord`类。
- 可以通过`get_record_list`方法获取指定记录ID的记录列表。
- 可以通过`get_record`方法根据记录ID获取单个记录。
- 可以通过`insert_record`方法插入新的记录。
- 可以通过`get_record_by_type`方法根据类型获取记录列表。

以上是对RunningRecord类的详细解释和使用说明。
***
