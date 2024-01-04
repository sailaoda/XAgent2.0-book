# ClassDef PlanEngine
**PlanEngine函数**：这个类的功能是计划引擎，用于执行计划任务。

PlanEngine类继承自BaseEngine类，它具有以下属性和方法：

- **属性**：
  - `toolserverifs`：ToolServerInterface对象，用于与工具服务器进行通信。
  - `agent`：PlanAgent对象，用于执行计划任务。
  - `generate_inital_plan_function`：用于生成初始计划的函数。
  - `plan_rectify_function`：用于修正计划的函数。
  - `exec_track`：PlanExecutionGraph对象，用于记录计划的执行轨迹。
  - `exec_node`：PlanExecutionNode对象，用于记录计划的执行节点。
  - `during_exec`：布尔值，表示是否正在执行计划。
  - `handling_task`：PlanNode对象，表示当前正在处理的任务。
  - `interrupt_during_exec`：布尔值，表示在执行计划过程中是否发生了中断。
  - `interrupt_msg_buffers`：中断消息的缓冲区，用于存储中断消息。

- **方法**：
  - `__init__(self, config=CONFIG)`：初始化PlanEngine对象。
  - `listen_interruption(self)`：监听中断消息的方法。
  - `step(self, handling_task: PlanNode, execution_engine: BaseEngine, **kwargs) -> PlanExecutionNode`：执行计划的一步，并返回执行结果。
  - `run(self, task: PlanNode, execution_engine: BaseEngine, **kwargs) -> PlanExecutionGraph`：执行计划并返回执行结果。
  - `plan_rectify(self, handling_task: PlanNode, interrupt_message: str = "") -> PlanExecutionNode`：修正计划的方法。
  - `execute(self, tool_call: ToolCall, handling_task: PlanNode, plan_tree: PlanTree) -> Tuple[PlanOperationStatusCode, str]`：执行工具调用的方法。

**注意**：
- PlanEngine类用于执行计划任务，它通过调用PlanAgent和ExecutionEngine来执行具体的任务。
- 在执行计划的过程中，可以发生中断，中断消息会被存储在中断消息缓冲区中，并在适当的时候进行处理。
- PlanEngine类还提供了修正计划的功能，可以根据用户的需求对计划进行调整和优化。

**输出示例**：
```python
plan_engine = PlanEngine(config=CONFIG)
execution_engine = ReActEngine(config=CONFIG)

task = PlanNode(
    name="User have set your goal, achieve it",
    goal="Find primes below 100",
)

result = await plan_engine.run(task, execution_engine=execution_engine)
print(result)
```
输出结果：
```
PlanExecutionGraph对象，记录了计划的执行轨迹和结果。
```
## FunctionDef __init__
**__init__函数**: 该函数的功能是初始化Plan类的实例。

该函数首先创建了一个ToolServerInterface的实例，并将其存储在self.toolserverifs属性中。ToolServerInterface可能是一个用于与工具服务器进行交互的接口类。

接着，函数调用了父类的__init__方法，将config配置和包含self.toolserverifs的列表作为参数传递，并且创建了一个PlanAgent实例作为agent参数。PlanAgent可能是一个负责计划任务的代理类。

此外，函数还从function_manager获取了两个函数模式，并将它们分别赋值给self.generate_inital_plan_function和self.plan_rectify_function属性。这些函数模式可能用于生成初始计划和调整计划的子任务操作。

函数还初始化了两个属性self.exec_track和self.exec_node，它们分别用于存储计划执行图和计划执行节点的实例。PlanExecutionGraph和PlanExecutionNode可能是用于跟踪计划执行过程的类。

最后，函数设置了一些用于中断处理的属性。self.during_exec是一个布尔值，用于标识是否处于执行过程中。self.handling_task用于存储当前正在处理的任务。self.interrupt_during_exec是一个布尔值，用于标识执行过程中是否发生了中断。self.interrupt_msg_buffers是一个列表，用于缓存中断期间的消息。

**注意**:
- 在使用这段代码时，需要确保function_manager已经定义并且提供了'generate_inital_plan'和'subtask_operations'这两个函数模式。
- PlanAgent和ToolServerInterface类需要被正确实现，因为它们是在__init__函数中创建实例的关键部分。
- 中断处理逻辑的实现细节需要根据实际的应用场景来完成，这里的属性只是提供了一个基本的框架。
- 在实际使用中，可能需要根据具体的配置和环境来调整config参数的默认值。
## AsyncFunctionDef listen_interruption
**listen_interruption 函数**: 此函数的功能是监听中断信号，并在执行过程中处理中断。

此函数是一个异步函数，它在一个无限循环中运行，每隔一秒钟检查一次是否有中断信号。如果在执行过程中检测到中断信号（`self.interrupt.is_set()`返回`True`），则执行以下步骤：

1. 设置`self.interrupt_during_exec`为`True`，表示在执行过程中发生了中断。
2. 记录中断日志，使用`recorder.log`函数，并将日志颜色设置为红色（`Fore.RED`）。
3. 等待3秒钟，可能是为了确保所有相关的处理都能够响应中断。
4. 获取中断消息，并将其封装为`InterruptMessage`对象，然后添加到`self.interrupt_msg_buffers`列表中。
5. 调用`self.agent.update_longterm_context`函数更新长期上下文，传入用户的中断消息。
6. 调用`self.plan_rectify`函数进行计划的修正，传入当前处理的任务和中断消息。
7. 清除中断信号，重置`self.interrupt`。
8. 将`self.interrupt_during_exec`设置为`False`，表示中断处理完成。

在项目中，`listen_interruption`函数被`XAgent/engines/interaction.py`文件中的`run`函数调用。在`run`函数中，首先会创建一个`plan_interrupt_listener`任务来运行`listen_interruption`函数。然后，`run`函数会使用`asyncio.gather`等待多个任务同时运行，包括状态检查、计划执行、中断处理和中断监听。如果在执行过程中发生了取消操作（`asyncio.CancelledError`），会记录取消日志，并取消所有相关的任务。

**注意**：
- `listen_interruption`函数设计为在后台持续运行，因此需要确保它不会阻塞其他并发运行的任务。
- 在使用此函数时，应确保相关的属性和方法（如`self.interrupt`、`self.interrupt_during_exec`、`recorder.log`、`self.agent.update_longterm_context`和`self.plan_rectify`）已经在类中正确定义和实现。
- 此函数中的`asyncio.sleep`调用是为了减少CPU的使用率，避免无限循环导致的资源浪费。调整睡眠时间可以根据实际需要进行优化。
- 在处理中断时，应考虑程序的健壮性和异常处理，确保中断后能够正确恢复或终止任务。
## AsyncFunctionDef step
**step函数**：这个函数的功能是执行一个任务并返回执行结果。

在这个函数中，首先将handling_task的状态设置为DOING，然后记录任务开始的日志。接着，将self.handling_task和self.during_exec设置为True，表示正在执行任务，并将execution_engine的general_query属性设置为self.general_query，以便在自我演化过程中使用。

如果execution_engine是PipelineEngine的实例，会尝试从数据库中检索与handling_task相关的pipeline，并选择一个存在的pipeline进行执行。执行结果存储在exec_result中。

如果execution_engine不是PipelineEngine的实例，直接将handling_task和其他参数传递给execution_engine的run方法，并将执行结果存储在exec_result中。

接下来，将exec_result中的所有工具调用添加到actions列表中，并创建一个PlanExecutionNode对象，将actions和handling_task的其他属性作为参数传递进去。

最后，将handling_task的状态设置为SUCCESS或FAIL，根据exec_result的状态决定。记录任务结束的日志，并在执行过程中处理中断。

在函数的最后，返回exec_node对象作为执行结果。

**注意**：在执行过程中，会监听中断，并在执行完成后停止监听。如果有中断消息缓冲，会将其添加到exec_node的interrupts列表中。如果exec_result需要进行计划修正，会调用plan_rectify方法进行修正。最后，会检查是否还有剩余的任务，如果没有则设置exec_node的end_node属性为True。

**输出示例**：假设exec_node的属性为{"actions": [...], "status": "SUCCESS", "pid": 123, "goal": "example"}，则函数的返回值为exec_node对象。
## AsyncFunctionDef run
**run函数**: 该函数的功能是执行计划引擎并返回结果节点。

run函数是XAgent项目中PlanEngine类的一个异步方法，它负责根据给定的任务节点（PlanNode）和执行引擎（BaseEngine），执行计划并构建执行图（PlanExecutionGraph）。该函数是计划引擎的核心执行逻辑，用于生成、执行和调整计划，直至达成目标或者计划执行失败。

函数首先进行懒加载初始化，然后根据传入的任务节点创建执行跟踪图，该图记录了计划的执行过程。接着，函数会根据配置决定是否从数据库中检索历史相似计划，并更新代理的工作上下文。如果配置允许，函数会生成初始计划并将其加载到执行跟踪图中。

接下来，函数进入一个循环，循环中不断获取下一个待处理的任务节点，调用step函数执行任务，并更新执行跟踪图。当没有更多的任务节点需要处理时，循环结束。

最后，函数会记录执行跟踪图的状态，总结执行过程，并根据配置决定是否在线上整合计划。函数返回执行跟踪图作为执行结果。

在项目中，run函数被interaction.py文件中的InteractionEngine类调用。在这里，run函数用于处理用户交互过程中的任务，生成计划并执行。InteractionEngine类首先创建一个初始任务节点，然后调用run函数开始执行计划。在执行过程中，还会处理中断和状态检查。

**注意**：
- 由于run函数是异步的，调用它时需要使用`await`关键字或在异步环境中运行。
- run函数的执行依赖于传入的任务节点和执行引擎，因此在调用前需要确保这些依赖项已正确初始化和配置。
- 函数中有一些TODO标记，这表示代码可能还在开发中，或者有一些功能尚未实现。

**输出示例**：
由于run函数返回的是PlanExecutionGraph对象，该对象记录了计划的执行过程，因此可能的返回值示例将是一个包含了计划树、节点和边的复杂对象。具体的结构将取决于执行过程和任务的复杂性。例如：

```python
PlanExecutionGraph(
    plan_tree=PlanTree(
        root=PlanNode(...),
        children=[...],
        ...
    ),
    begin_node=PlanExecutionNode(...),
    end_node=PlanExecutionNode(...),
    status=EngineExecutionStatusCode.SUCCESS,
    ...
)
```

在这个示例中，`PlanExecutionGraph`对象包含了计划树的根节点、开始节点、结束节点以及执行状态等信息。
## AsyncFunctionDef plan_rectify
**plan_rectify函数**：该函数的功能是根据任务代理的建议迭代地修正计划。

该函数的代码逻辑如下：

1. 首先，记录日志，提示开始迭代地根据任务代理的建议修正计划。
2. 初始化变量`modify_steps`为0，`max_step`为配置文件中的最大计划修正链长度。
3. 如果`interrupt_message`不为空，则将建议列表`suggestions`设置为固定的一条提示信息，表示用户提供了额外的信息，需要根据用户的需求修正计划。否则，将建议列表`suggestions`设置为任务节点中的建议列表。
4. 调用`get_file_system_structure`函数获取文件系统结构。
5. 进入循环，当`modify_steps`小于`max_step`时执行以下操作：
   - 记录日志，提示正在修正计划（仍在循环中）。
   - 调用`agent.update_working_context`函数更新工作上下文，传入基本信息，包括文件系统结构、中断信息和任务执行结论。
   - 调用`agent.step`函数进行一步操作，传入查询、修正建议、计划概览、任务ID和最大步数等参数，以及工具选择，指定函数类型和函数名称为`plan_rectify_function`。
   - 遍历工具调用列表，对每个工具调用执行操作，并记录日志。如果工具调用的状态不是`MODIFY_SUCCESS`，则记录修正失败的日志。
   - 调用`agent.update_tool_calls_output`函数更新工具调用的输出。
6. 记录日志，提示修正计划已退出。
7. 记录日志，显示修正后的计划。

**注意**：在使用该代码时需要注意以下几点：
- 需要根据实际情况调整日志记录的颜色和格式。
- 需要根据实际情况调整循环的终止条件。
- 需要根据实际情况调整函数调用的参数和返回值。

该函数在项目中的调用情况如下：

- 文件路径：XAgent/engines/plan.py
- 调用代码片段：
```python
await self.plan_rectify(
    handling_task=self.handling_task,
    interrupt_message=interrupt_msg.message,
)
```
- 调用说明：在`listen_interruption`函数中，当发生中断时，调用`plan_rectify`函数修正计划。
## AsyncFunctionDef execute
**execute函数**：这个函数的作用是执行一个工具调用。

该函数接受三个参数：tool_call（工具调用对象），handling_task（当前处理的任务节点），plan_tree（计划树）。

函数内部首先根据tool_call的参数创建一个PlanOperation对象。然后根据plan_operation的operation字段进行不同的操作。

- 如果operation是'exit'，表示退出计划修正过程，函数会将tool_call的状态设置为PLAN_REFINE_EXIT，并返回相应的输出信息。
- 如果operation是其他值，表示执行其他操作。函数会根据target_task_id查找对应的任务节点，如果找不到则将tool_call的状态设置为TARGET_SUBTASK_NOT_FOUND，并返回相应的输出信息。
- 如果target_task_id小于handling_task的pid，表示无法修改之前的任务节点，函数会将tool_call的状态设置为MODIFY_FORMER_PLAN，并返回相应的输出信息。
- 如果operation是'split'，表示拆分任务。函数会检查目标任务的深度是否超过最大深度限制，如果超过则将tool_call的状态设置为OTHER_ERROR，并返回相应的输出信息。否则，函数会根据subtasks创建新的任务节点，并将其添加为目标任务的子节点。最后，函数将目标任务的状态设置为SPLIT，将tool_call的状态设置为MODIFY_SUCCESS，并返回相应的输出信息。
- 如果operation是'add'，表示添加任务。函数会检查目标任务的深度是否小于等于1，如果是则将tool_call的状态设置为OTHER_ERROR，并返回相应的输出信息。如果目标任务的子节点数量加上subtasks的数量超过了最大宽度限制，则将tool_call的状态设置为OTHER_ERROR，并返回相应的输出信息。否则，函数会根据subtasks创建新的任务节点，并将其添加到目标任务的父节点的指定位置。最后，函数将tool_call的状态设置为MODIFY_SUCCESS，并返回相应的输出信息。
- 如果operation是'delete'，表示删除任务。函数会检查目标任务的状态是否为TODO，如果不是则将tool_call的状态设置为OTHER_ERROR，并返回相应的输出信息。否则，函数会将目标任务从父节点的子节点列表中移除，并将其父节点设置为None。最后，函数将tool_call的状态设置为MODIFY_SUCCESS，并返回相应的输出信息。
- 如果operation是其他值，表示操作未知，函数会将tool_call的输出信息设置为相应的提示信息，并将tool_call的状态设置为PLAN_OPERATION_NOT_FOUND。

最后，函数返回tool_call的状态和输出信息。

**注意**：使用该代码时需要注意以下几点：
- 需要传入一个ToolCall对象作为参数tool_call，该对象包含了工具调用的相关信息。
- 需要传入一个PlanNode对象作为参数handling_task，该对象表示当前处理的任务节点。
- 需要传入一个PlanTree对象作为参数plan_tree，该对象表示整个计划树的结构。

**输出示例**：假设函数执行成功，返回的元组为(PlanOperationStatusCode.MODIFY_SUCCESS, "Split the task 1 successfully.")。
***
