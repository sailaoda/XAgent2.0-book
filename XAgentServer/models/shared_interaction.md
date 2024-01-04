# ClassDef SharedInteractionBase
**SharedInteractionBase函数**：这个类的功能是表示共享交互的基本信息。

SharedInteractionBase是一个抽象基类，用于表示共享交互的基本信息。它具有以下属性：

- interaction_id：交互ID，表示交互的唯一标识符。
- user_name：用户名，表示创建交互的用户。
- create_time：创建时间，表示交互的创建时间。
- update_time：更新时间，表示交互的最后更新时间。
- description：描述，表示交互的描述信息。
- agent：代理，表示交互所属的代理。
- mode：模式，表示交互的运行模式。
- is_deleted：是否已删除，表示交互是否已被删除。
- star：星级评价，表示交互的星级评价。
- is_audit：是否已审核，表示交互是否已经通过审核。

SharedInteractionBase类具有以下方法：

- to_dict(include=None, exclude=None)：将SharedInteractionBase对象转换为字典形式。可以通过include和exclude参数指定要包含或排除的属性。
- to_json()：将SharedInteractionBase对象转换为JSON字符串。
- from_db(interaction)：从数据库对象中创建SharedInteractionBase对象。

**注意**：在使用SharedInteractionBase类时，需要提供交互的基本信息，并可以通过调用to_dict方法将对象转换为字典形式或调用to_json方法将对象转换为JSON字符串。

**输出示例**：
```python
{
    "interaction_id": "123456",
    "user_name": "John",
    "create_time": "2022-01-01 10:00:00",
    "update_time": "2022-01-01 12:00:00",
    "description": "这是一个示例交互",
    "agent": "Agent1",
    "mode": "模式1",
    "is_deleted": False,
    "star": 0,
    "is_audit": False
}
```
## FunctionDef __init__
**__init__函数**: 该函数的作用是初始化SharedInteraction类的实例。

该__init__函数是SharedInteraction类的构造函数，用于创建新的SharedInteraction对象实例。构造函数接收多个参数，用于设置对象的属性。

- `interaction_id` (str): 交互ID，用于唯一标识一个交互实例。
- `user_name` (str): 用户名，表示创建该交互的用户。
- `create_time` (str): 创建时间，记录交互被创建的时间。
- `update_time` (str): 更新时间，记录交互最后一次被更新的时间。
- `description` (str): 描述，提供关于交互内容的描述信息。
- `agent` (str, 可选): 代理，表示参与交互的代理，默认为空字符串。
- `mode` (str, 可选): 模式，表示交互的模式，默认为空字符串。
- `is_deleted` (bool, 可选): 是否已删除标志，表示该交互是否已被删除，默认为False。
- `star` (int, 可选): 星级，用于评价交互的星级，默认为0。
- `is_audit` (bool, 可选): 是否审核标志，表示该交互是否已经过审核，默认为False。

在创建SharedInteraction对象时，必须提供`interaction_id`、`user_name`、`create_time`、`update_time`和`description`这五个参数，而`agent`、`mode`、`is_deleted`、`star`和`is_audit`则是可选参数，如果不提供，将使用默认值。

**注意**：在使用该构造函数时，需要确保提供的`interaction_id`是唯一的，以避免交互实例之间的冲突。同时，`create_time`和`update_time`应该是有效的时间字符串，以保证时间记录的准确性。对于`star`参数，应该是一个非负整数。如果需要对交互进行逻辑删除，可以将`is_deleted`设置为True，而不是从数据库中物理删除记录。
## FunctionDef to_dict
**to_dict函数**: 此函数的作用是将对象的属性转换为字典形式。

`to_dict`函数是一个对象方法，它用于将对象的属性转换成一个字典，这个字典可以包含所有的属性，也可以通过`include`和`exclude`参数来定制包含或排除某些特定的属性。这种转换通常用于将对象的数据准备成一种更易于处理和传输的格式，比如在Web开发中，将数据转换为JSON格式以响应HTTP请求。

具体来说，`to_dict`函数首先创建了一个包含对象所有属性的字典`data`。然后，如果提供了`include`参数，函数会过滤字典，只保留`include`中指定的键。如果提供了`exclude`参数，函数会过滤字典，排除`exclude`中指定的键。最后，函数返回处理后的字典。

在项目中，`to_dict`函数被用于以下场景：

1. 在`XAgentServer/database/interface/interaction.py`文件中的`search_many_shared`方法里，用于获取共享交互的列表，并排除了`record_dir`和`is_deleted`属性。
2. 同样在`XAgentServer/database/interface/interaction.py`文件中的`add_share`方法里，用于将共享交互的数据添加到数据库中。
3. 在`XAgentServer/models/shared_interaction.py`文件中的`to_json`方法里，用于将对象转换为JSON格式的字符串。

**注意**：
- 在使用`to_dict`函数时，需要注意`include`和`exclude`参数不能同时使用，因为它们会相互冲突。
- 函数返回的字典可以直接用于构造JSON响应或者添加到数据库中，这取决于调用它的上下文。

**输出示例**：
假设一个`SharedInteraction`对象具有以下属性：
```
interaction_id: 1,
user_name: "张三",
create_time: "2021-01-01T12:00:00",
update_time: "2021-01-02T12:00:00",
description: "示例交互",
agent: "XAgent",
mode: "自动",
is_deleted: False,
star: 5,
is_audit: True
```
调用`to_dict()`函数可能会返回如下字典：
```python
{
    "interaction_id": 1,
    "user_name": "张三",
    "create_time": "2021-01-01T12:00:00",
    "update_time": "2021-01-02T12:00:00",
    "description": "示例交互",
    "agent": "XAgent",
    "mode": "自动",
    "is_deleted": False,
    "star": 5,
    "is_audit": True
}
```
如果调用`to_dict(exclude=["is_deleted"])`，则返回的字典将不包含`is_deleted`键：
```python
{
    "interaction_id": 1,
    "user_name": "张三",
    "create_time": "2021-01-01T12:00:00",
    "update_time": "2021-01-02T12:00:00",
    "description": "示例交互",
    "agent": "XAgent",
    "mode": "自动",
    "star": 5,
    "is_audit": True
}
```
## FunctionDef to_json
**to_json函数**: 该函数的功能是将对象转换成JSON格式的字符串。

该`to_json`函数是一个对象的方法，用于将对象的数据转换成JSON（JavaScript Object Notation）格式的字符串。JSON是一种轻量级的数据交换格式，易于人阅读和编写，同时也易于机器解析和生成。

在这个函数中，首先调用了对象的`to_dict`方法，该方法的作用是将对象的属性转换为一个字典（键值对的集合）。然后，使用`json.dumps`函数将这个字典转换为一个格式化的JSON字符串。`json.dumps`函数接受几个参数：

- 第一个参数是要转换的字典。
- `indent`参数设置了JSON字符串的缩进级别，这里设置为2，意味着每个层级的缩进是两个空格，这样生成的JSON字符串更易于阅读。
- `ensure_ascii`参数设置为`False`，意味着在JSON字符串中可以包含非ASCII字符，例如中文等。如果不设置或设置为`True`，所有非ASCII字符都会被转义成`\uXXXX`格式。

**注意**：在使用`to_json`函数时，需要确保对象的`to_dict`方法正确实现，能够将对象的属性转换为字典。此外，所有属性的值都需要是JSON支持的数据类型，例如字符串、数字、数组（在Python中为列表）或其他对象（字典）等。如果对象的属性中包含不支持的数据类型，如Python的日期时间对象，需要在转换为字典之前将其转换为支持的格式，例如字符串。

**输出示例**:
```json
{
  "name": "张三",
  "age": 30,
  "skills": ["Python", "JavaScript"]
}
```
在这个示例中，假设对象有三个属性：`name`、`age`和`skills`。`to_dict`方法将这些属性转换为字典，然后`to_json`方法将字典转换为上面显示的格式化JSON字符串。
## FunctionDef from_db
**from_db函数**: 该函数的功能是将数据库中的交互记录对象转换为SharedInteractionBase类的实例。

该`from_db`函数是`SharedInteractionBase`类的一个类方法，它的作用是将从数据库查询得到的交互记录对象（通常是ORM模型的实例）转换为`SharedInteractionBase`类的实例。这个过程通常被称为反序列化，即将数据库中存储的数据结构转换为Python中的对象。

在这个函数中，我们接收一个名为`interaction`的参数，它代表从数据库中查询到的交互记录。函数内部通过访问`interaction`对象的属性，将这些属性值作为参数传递给`SharedInteractionBase`类的构造函数，从而创建一个新的`SharedInteractionBase`实例。

具体来说，`from_db`函数会获取以下属性：
- `interaction_id`: 交互记录的唯一标识符。
- `user_name`: 与交互记录相关联的用户名。
- `create_time`: 交互记录的创建时间。
- `update_time`: 交互记录的更新时间。
- `description`: 交互记录的描述信息。
- `agent`: 交互记录中使用的代理。
- `mode`: 交互记录的模式。
- `is_deleted`: 标记交互记录是否已被删除。
- `star`: 交互记录的星级评分。
- `is_audit`: 标记交互记录是否已经过审核。

在项目中，`from_db`函数被用于两个场景：
1. 在`search_many_shared`方法中，用于从数据库分页查询多个共享交互记录，并将每条记录转换为`SharedInteractionBase`实例，然后转换为字典格式输出。
2. 在`get_shared_interaction`方法中，用于通过交互记录的ID查询单个共享交互记录，并将查询结果转换为`SharedInteractionBase`实例。

**注意**：
- 在使用`from_db`函数时，需要确保传入的`interaction`对象是一个有效的数据库查询结果，且包含所有必要的属性。
- 如果数据库中的记录已被删除或不存在，`from_db`函数应当能够正确处理这种情况，避免抛出异常。

**输出示例**：
假设数据库中有一条交互记录，其属性如下：
```python
interaction = {
    'interaction_id': '12345',
    'user_name': '张三',
    'create_time': '2021-01-01T00:00:00',
    'update_time': '2021-01-02T00:00:00',
    'description': '这是一个示例交互',
    'agent': 'XAgent',
    'mode': '模式A',
    'is_deleted': False,
    'star': 5,
    'is_audit': True
}
```
调用`from_db`函数后，将返回一个`SharedInteractionBase`实例，其属性与输入的交互记录相对应。
***
