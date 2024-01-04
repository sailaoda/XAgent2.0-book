# ClassDef ToolServerNode
**ToolServerNode 类**

ToolServerNode 类表示数据库中的一个节点。

**属性**：
- id: str，节点的唯一标识符。
- short_id: str，节点的短标识符。
- manager_id: str，节点所属的管理器的标识符。
- status: str，默认值为"Created"，节点的状态。
- health: str，默认值为"Unknown"，节点的健康状态。
- last_req_time: datetime，节点的最后请求时间。
- ip: str，默认值为"0.0.0.0"，节点的 IP 地址。
- port: int，默认值为0，节点的端口号。

**ToolServerNode 类的功能**：
ToolServerNode 类用于表示数据库中的一个节点。它包含了节点的各种属性，如唯一标识符、短标识符、所属的管理器标识符、状态、健康状态、最后请求时间、IP 地址和端口号。

**注意**：
- ToolServerNode 类用于表示数据库中的节点，可以通过访问类的属性来获取节点的信息。
- 节点的状态和健康状态可以通过修改类的属性来更新。
- ToolServerNode 类的实例可以在代码中被创建和操作，以便对节点进行管理和控制。

**ToolServerNode 类的调用情况**：
ToolServerNode 类在以下文件中被调用：
- ToolServer/ToolServerManager/main.py
- ToolServer/ToolServerManager/node_checker.py

在 ToolServer/ToolServerManager/main.py 文件中，ToolServerNode 类被用于创建和管理节点。在函数 lifespan 中，通过调用 init_beanie 函数来初始化数据库，并将 ToolServerNode 类作为 document_models 参数传递给该函数，以便在数据库中创建节点表。在函数 read_cookie_info 中，通过创建 ToolServerNode 类的实例来添加节点到数据库，并等待节点启动。在函数 reconnect_session、close_session 和 release_session 中，通过查询数据库获取节点信息，并根据节点的状态进行相应的操作。在函数 route_to_node 中，通过查询数据库获取节点信息，并将请求转发到指定的节点。

在 ToolServer/ToolServerManager/node_checker.py 文件中，ToolServerNode 类被用于更新节点的状态。在函数 update_node_status 中，通过查询 Docker 获取节点的状态和健康状态，并更新到数据库中的节点对象。

**注意**：
- ToolServerNode 类在 ToolServer/ToolServerManager/main.py 文件中被用于创建和管理节点。
- ToolServerNode 类在 ToolServer/ToolServerManager/node_checker.py 文件中被用于更新节点的状态。
- ToolServerNode 类的实例可以在代码中被创建和操作，以便对节点进行管理和控制。
***
# ClassDef NodeChecker
**NodeChecker功能**：NodeChecker类用于表示节点检查器，用于检查节点的状态。

NodeChecker类具有以下属性：
- manager_id：节点检查器的管理器ID，类型为字符串，必填。
- pid：节点检查器的进程ID，类型为整数，可选，默认为None。
- interval：节点检查器的检查间隔，类型为浮点数。

在项目中，NodeChecker类被以下代码调用：

在ToolServer/ToolServerManager/main.py文件中的lifespan函数中：
- 通过调用init_beanie函数初始化数据库配置，其中document_models参数包含ToolServerNode和NodeChecker两个模型。
- 如果CONFIG['builtin_monitor']为True，则创建一个子进程来更新节点状态。
- 检查是否已经有一个NodeChecker实例在运行，如果存在，则删除该实例。
- 如果不存在NodeChecker实例，则创建一个NodeChecker实例并启动子进程。
- 注册路径到节点。
- 在函数结束时进行清理操作，包括删除NodeChecker实例和停止所有管理的节点。

在ToolServer/ToolServerManager/node_checker.py文件中的check_nodes_status_loop函数中：
- 一个无限循环，用于检查节点的状态。
- 在每次迭代之前等待1秒。
- 检查节点的状态，并在发生异常时打印异常信息。
- 检查节点检查器是否仍然存活，如果不存在，则停止循环。

**注意**：在使用NodeChecker类时，需要注意以下几点：
- manager_id属性是必填的，需要提供一个唯一的管理器ID。
- pid属性是可选的，如果不提供，则默认为None。
- interval属性表示节点检查器的检查间隔，可以根据实际需求进行调整。
- 在使用NodeChecker类之前，需要先调用init_beanie函数进行数据库的初始化配置。
- 在使用NodeChecker类时，需要先创建一个NodeChecker实例，并保存到数据库中。
- 在使用NodeChecker类时，需要注意检查节点检查器是否已经存在，避免重复创建。
- 在使用NodeChecker类时，需要注册路径到节点，以便将请求路由到相应的节点。
- 在使用NodeChecker类时，需要进行清理操作，包括删除NodeChecker实例和停止所有管理的节点。

以上是对NodeChecker类的详细解释和分析。

**注意**：以上内容仅供参考，具体使用时请根据实际情况进行调整和修改。
***
