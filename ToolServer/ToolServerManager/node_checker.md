# AsyncFunctionDef update_node_status
**update_node_status函数**: 此函数的功能是更新数据库中节点的状态。

此函数是一个异步函数，用于更新ToolServerNode实例（即节点）的状态。它首先尝试从Docker客户端获取与节点ID对应的容器实例。如果容器不存在（即docker.errors.NotFound异常被抛出），则会从数据库中删除该节点，并记录日志信息。如果在尝试获取容器信息时遇到API错误（即docker.errors.APIError异常），则记录警告日志并返回。

如果成功获取到容器实例，函数将继续获取容器的状态，并更新节点的IP地址和端口信息。这些信息是从容器的属性中提取的，其中IP地址是基于配置文件中指定的网络来获取的。

如果容器的状态与节点当前的状态不同，函数会更新节点的状态，并记录状态变更的日志信息。如果配置文件中启用了健康检查，函数还会检查并更新节点的健康状态。

最后，函数会调用节点的replace方法，将更新后的节点信息保存到数据库中。

此外，如果节点的状态为"running"，但自上次请求以来已经超过了配置中指定的空闲时间，则会停止该节点的容器，并记录日志信息。

**调用情况**:
- 在`ToolServer/ToolServerManager/main.py`文件中的`close_session`函数中调用`update_node_status`，用于在关闭节点会话时更新节点状态。
- 在`ToolServer/ToolServerManager/main.py`文件中的`release_session`函数中调用`update_node_status`，用于在释放节点会话时更新节点状态。
- 在`ToolServer/ToolServerManager/node_checker.py`文件中的`check_nodes_status`函数中调用`update_node_status`，用于检查所有节点的状态，并在Docker中不存在的节点将被从数据库中删除。

**注意**:
- 使用此函数时，需要确保传入的`node`参数是一个有效的ToolServerNode实例。
- 函数依赖于外部的Docker客户端和配置文件（CONFIG），因此在调用此函数之前，需要确保这些依赖项已正确设置。
- 由于函数执行了异步数据库操作和Docker操作，调用此函数的上下文也应该是异步的。

**输出示例**:
由于此函数没有返回值，它主要通过日志来报告其操作结果。以下是一些可能出现在日志中的信息示例：
- "Node deleted from db: node_id(not in docker)"，表示节点在Docker中未找到，并已从数据库中删除。
- "Failed to get node info from docker: node_id"，表示无法从Docker获取节点信息。
- "Node node_short_id status updated: old_status -> new_status"，表示节点的状态已更新。
- "Node node_short_id health updated: old_health -> new_health"，表示节点的健康状态已更新。
- "Stopping node: node_id due to idling time used up"，表示由于节点空闲时间耗尽，已停止节点。
***
# AsyncFunctionDef check_nodes_status
**check_nodes_status函数**: 此函数的功能是检查所有现有节点的状态。

此函数`check_nodes_status`是一个异步函数，其主要作用是遍历数据库中记录的所有节点，并检查它们的状态。它从数据库（如sqlite3或mongodb）中选择所有节点，并对每个节点调用`update_node_status`函数来更新其状态。

在检查节点状态时，如果发现某个节点在Docker中不存在，则会将其从数据库中删除。这个过程涉及到与Docker的交互，可能会引发两种异常：`docker.errors.NotFound`和`docker.errors.APIError`。`docker.errors.NotFound`异常表示在Docker中没有找到对应的节点，而`docker.errors.APIError`异常表示尝试从Docker获取节点信息时失败。

此函数被`check_nodes_status_loop`函数在`ToolServer/ToolServerManager/node_checker.py`文件中调用。`check_nodes_status_loop`函数是一个无限循环，它会定期调用`check_nodes_status`函数来检查节点状态，并在每次迭代之间等待一定时间（默认为1秒，但可以通过配置文件`CONFIG['node']`中的`health_check_interval`键值来调整等待时间）。

在`check_nodes_status_loop`函数中，如果在检查节点状态的过程中发生异常，会捕获异常并打印堆栈跟踪信息。此外，该函数还会检查`NodeChecker`对象是否仍然存在，如果不存在，则日志记录器会记录信息，并停止节点状态检查器。

**注意**:
- 由于`check_nodes_status`函数是异步的，调用此函数时需要在异步环境中使用`await`关键字。
- 在使用此函数时，需要确保数据库和Docker环境已正确配置，并且可以正常访问。
- 异常处理是必要的，因为与Docker的交互可能会失败，需要妥善处理这些情况以避免程序崩溃。
- 在实际部署时，应注意`check_nodes_status_loop`函数中的无限循环可能会导致函数永远不会返回，因此需要确保有适当的退出机制。
***
# AsyncFunctionDef check_nodes_status_loop
**check_nodes_status_loop函数**: 该函数的功能是持续检查节点的状态，并在每次迭代之前等待一定时间。

该`check_nodes_status_loop`函数是一个异步函数，它接收一个`NodeChecker`对象作为参数。该函数的主要目的是创建一个无限循环，不断地检查节点的状态。在每次迭代之前，它会根据配置文件中的`health_check_interval`值（默认为1秒）暂停执行。

函数的执行流程如下：
1. 函数开始时，会记录下传入的`NodeChecker`对象的`id`，并在日志中记录节点状态检查器已启动的信息。
2. 进入无限循环，在循环中：
   - 尝试调用`check_nodes_status()`函数来检查节点状态。
   - 如果在检查过程中发生异常，会捕获异常并打印堆栈跟踪信息。
   - 检查传入的`NodeChecker`对象是否仍然存在于数据库中。如果不存在，说明检查器已经停止，函数将记录日志并返回，结束循环。
   - 如果检查器仍然存在，函数将根据配置中的间隔时间暂停执行，然后继续下一次迭代。

在项目中，`check_nodes_status_loop`函数被`ToolServer/ToolServerManager/main.py`文件中的`lifespan`异步函数调用。在`lifespan`函数中，首先进行了数据库初始化和节点状态更新子进程的创建。然后，如果配置中启用了内置监控，它会检查是否已经有一个`NodeChecker`在运行。如果没有，它会创建一个新的`NodeChecker`实例，并启动一个新的线程来运行`check_nodes_status_loop`函数，以监控节点状态。

**注意**：
- 在使用`check_nodes_status_loop`函数时，需要确保传入的`NodeChecker`对象是有效的，并且数据库连接是正常的，因为函数中涉及到了数据库的查询和更新操作。
- 由于该函数设计为无限循环，调用者需要确保在合适的时机停止循环，例如在应用关闭时，需要清理并删除对应的`NodeChecker`实例，并停止所有相关的节点。
- 函数内部使用了`asyncio.sleep`来控制循环的频率，这个值应该根据实际需求进行配置。

**输出示例**：由于该函数是一个无限循环，它本身不会返回任何值。但是，它会在日志中输出相关的信息，例如“Nodes status checker started.”表示检查器已经启动，“Nodes status checker stopped.”表示检查器已经停止。如果在检查节点状态时发生异常，还会在日志中打印出异常的堆栈跟踪信息。
***
