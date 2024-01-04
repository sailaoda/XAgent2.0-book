# ClassDef ReActEngine
**ReActEngine函数**: 这个类的功能是实现ReAct引擎，它是BaseEngine的子类，用于执行计划并返回执行结果。

ReActEngine类有以下几个重要的方法和属性：

- `__init__(self, config=CONFIG)`: 构造函数，用于初始化ReActEngine对象。它接受一个可选的配置参数config，默认值为CONFIG。在构造函数中，它创建了ToolServerInterface对象和BuiltInInterface对象，并初始化了ToolAgent对象。然后调用父类BaseEngine的构造函数，传递配置参数和工具接口，以及ToolAgent对象。最后，它初始化了一些其他属性，如self.finish_tool_call、self.exec_node和self.exec_track。

- `async def step(self, task: PlanNode, plan_tree: PlanTree, tools: list[dict] = None, tool_choice: dict = None, **kwargs) -> ReActExecutionNode`: 这个方法用于执行一个步骤并返回执行结果。它接受以下参数：
  - task: PlanNode对象，表示当前任务节点。
  - plan_tree: PlanTree对象，表示整个计划树。
  - tools: 可选参数，表示可用的工具列表，默认为None。
  - tool_choice: 可选参数，表示用户选择的工具，默认为None。
  - kwargs: 其他可选参数。

  在方法内部，它首先将任务的状态设置为DOING。然后，它通过调用ToolAgent对象的step方法来执行工具调用，并返回回复和工具调用列表。根据工具调用的类型，它可能会执行不同的操作，如任务提交、执行工具等。最后，它更新工具调用的输出，并返回ReActExecutionNode对象。

- `async def run(self, task: PlanNode, plan_tree: PlanTree, plan_interruption_vec: int, **kwargs) -> ReActExecutionGraph`: 这个方法用于执行引擎并返回结果节点。它接受以下参数：
  - task: PlanNode对象，表示当前任务节点。
  - plan_tree: PlanTree对象，表示整个计划树。
  - plan_interruption_vec: 表示计划中断向量的整数值。
  - kwargs: 其他可选参数。

  在方法内部，它首先调用lazy_init方法进行懒加载初始化。然后，它创建一个ReActExecutionGraph对象，并设置开始节点。接下来，它更新长期上下文，并根据配置中的设置检索工具。然后，它进入一个循环，直到节点为结束节点。在循环中，它调用step方法执行一个步骤，并将返回的节点添加到执行图中。最后，它设置任务的结论，并返回执行图。

- `def task_submit(self, task: PlanNode, tool_call: ToolCall)`: 这个方法用于提交任务。它接受两个参数：
  - task: PlanNode对象，表示当前任务节点。
  - tool_call: ToolCall对象，表示工具调用。

  在方法内部，它创建一个TaskSubmission对象，并将任务的提交信息设置为TaskSubmission对象。然后，它更新工具调用的状态和输出，并返回任务的状态和工具调用的输出。

**注意**: 使用这个类时需要注意以下几点：
- 在初始化ReActEngine对象时，需要传递一个可选的配置参数config。
- 在执行步骤时，需要传递任务节点和计划树，并可以选择传递工具列表和工具选择。
- 在执行引擎时，需要传递任务节点、计划树和计划中断向量。

**输出示例**:
```python
engine = ReActEngine()
task = PlanNode(name="User have set your goal, achieve it", goal="Find primes below 100")
plan_tree = PlanTree()
result = await engine.run(task, plan_tree, plan_interruption_vec=0)
print(result)
```
输出:
```
<ReActExecutionGraph object at 0x7f9a2e5a9f10>
```
## FunctionDef __init__
**__init__函数**: 该函数的功能是初始化ReAct引擎的实例。

该函数是ReAct引擎类的构造函数，用于创建一个新的ReAct引擎实例，并初始化其内部状态和依赖关系。具体来说，该函数执行以下操作：

1. 接收一个可选的配置对象`config`，如果没有提供，则使用默认的`CONFIG`配置。
2. 创建一个`ToolServerInterface`实例，并将其赋值给成员变量`self.toolserverif`。这个接口用于与工具服务器进行交互。
3. 创建一个`BuiltInInterface`实例，并将其赋值给成员变量`self.builtinif`。这个接口提供了内置工具的访问。
4. 创建一个`ToolAgent`实例，并将其赋值给成员变量`self.agent`。这个代理用于执行工具调用和管理工具的执行。
5. 调用基类的构造函数`super().__init__`，传入配置对象`config`，工具接口列表（包含`self.toolserverif`和`self.builtinif`），以及代理`self.agent`。
6. 在成员变量`self.finish_tool_call`中存储一个字符串值`'subtask_submit'`，该值用于标识工具调用完成时的操作。
7. 声明两个成员变量`self.exec_node`和`self.exec_track`，分别用于存储执行节点和执行跟踪信息。这两个变量的类型分别是`ReActExecutionNode`和`ReActExecutionGraph`，但在构造函数中并未初始化，通常会在后续的执行流程中被赋值。

**注意**：
- 在使用该构造函数创建ReAct引擎实例时，可以根据需要传入自定义的配置对象`config`，以覆盖默认配置。
- `ToolServerInterface`和`BuiltInInterface`是与工具服务器和内置工具交互的关键接口，确保它们被正确初始化并传入基类构造函数是非常重要的。
- `self.finish_tool_call`的值应与ReAct引擎的内部逻辑保持一致，以确保工具调用完成后能够正确地执行后续操作。
- `self.exec_node`和`self.exec_track`是执行过程中的关键组件，它们将在ReAct引擎的执行流程中被赋值和使用。开发者在使用ReAct引擎时，应当了解这两个变量的作用和使用时机。
## AsyncFunctionDef step
**step函数**：此函数的功能是执行一个步骤并返回执行结果。

在这个函数中，首先将任务的状态设置为"DOING"。然后记录一条日志，表示开始执行REACT引擎的步骤，并显示相关信息。

接下来，通过调用get_file_system_structure()函数获取文件系统结构，并将结果赋值给file_archi变量。然后初始化一个空字符串observation。

在预处理阶段，创建一个ReActExecutionNode对象，并将其赋值给self.exec_node变量。

如果存在中断信号，将记录一条日志表示中断，并设置工具选择和工具列表，以处理中断情况。

接着，通过调用agent的update_working_context()函数更新工作环境的基本信息和观察结果。

如果存在工具选择，根据不同的选择，创建相应的系统消息。

然后，调用agent的step()函数执行步骤，并获取回复和工具调用列表。

遍历工具调用列表，根据工具调用的名称执行相应的操作。

更新工具调用的输出结果。

如果存在中断信号，检查是否有中断响应，并根据中断的类型进行相应的处理。

最后，将工具调用列表和中断信息添加到执行节点中，并返回执行节点。

**注意**：在执行步骤时，会根据不同的情况处理中断信号，并更新工具调用的输出结果。

**输出示例**：假设执行节点的输出结果为：
```
{
  "end_node": true,
  "interrupts": [],
  "tool_calls": [
    {
      "name": "tool1",
      "arguments": {
        "arg1": "value1"
      }
    },
    {
      "name": "tool2",
      "arguments": {
        "arg2": "value2"
      }
    }
  ]
}
```
## AsyncFunctionDef run
**run函数**: 该函数的功能是执行引擎并返回结果节点。

该`run`函数是一个异步函数，它是在一个基于事件循环的异步环境中执行的，通常用于处理并发任务。函数接收一个任务节点（`task`），一个计划树（`plan_tree`），一个计划中断向量（`plan_interruption_vec`）以及其他关键字参数（`**kwargs`），并返回一个`ReActExecutionGraph`对象，该对象代表了执行过程中的节点和边的图。

函数首先通过`await self.lazy_init(self.config)`进行懒加载初始化。然后，它创建一个`TaskNode`对象，并将其设置为执行跟踪图的起始节点。接着，函数根据配置中的最大工具检索次数，可能会检索与任务目标相关的工具。

函数通过`await self.agent.update_longterm_context(...)`更新代理的长期上下文，这包括用户消息、可用工具和当前目标。

接下来，函数进入一个循环，直到当前节点被标记为结束节点。在循环中，函数每隔2秒执行一次`await asyncio.sleep(2)`以避免过快地循环。然后，它调用`await self.step(...)`来执行下一步操作，并将返回的新节点添加到执行跟踪图中。

当任务完成时，函数会获取任务结论，并根据结论的状态设置执行跟踪图的状态。如果执行成功，并且配置允许在线内部整合，则会调用`execution_consolidator.extract_pipeline(...)`来提取执行流程。如果配置允许保存内部状态，则会调用`execution_consolidator.save_trace(...)`来保存执行跟踪。

最后，函数返回执行跟踪图`self.exec_track`。

**注意**：
- 由于`run`函数是异步的，调用它时需要在异步环境中使用`await`。
- 函数内部使用了多个异步调用，如`self.lazy_init`、`self.retrieve_tools`、`self.agent.update_longterm_context`和`self.step`，这意味着这些操作可能会涉及I/O操作，例如网络请求或数据库查询。
- 函数中的`asyncio.sleep(2)`是为了控制循环的速率，避免过快地进行无用的循环迭代。
- 函数的执行依赖于配置参数，如`self.config.execution.max_tool_retrieve_count`和`self.config.execution.max_chain_length`，这些参数需要在使用前正确配置。
- 函数返回的`ReActExecutionGraph`对象包含了执行过程中所有节点和边的信息，可以用于后续的分析或调试。

**输出示例**：
```python
# 假设的返回值示例
ReActExecutionGraph(
    begin_node=<TaskNode object>,
    end_node=<TaskNode object>,
    nodes=[<ReActExecutionNode object>, ...],
    edges=[(<TaskNode object>, <ReActExecutionNode object>), ...],
    status=EngineExecutionStatusCode.SUCCESS
)
```
在这个示例中，`ReActExecutionGraph`对象包含了起始节点、结束节点、所有执行过程中的节点列表、节点之间的边列表以及执行的状态。
## FunctionDef task_submit
**task_submit函数**：此函数的功能是将任务提交给工具调用。

该函数接受两个参数：task（计划节点）和tool_call（工具调用）。首先，根据tool_call的参数创建一个TaskSubmission对象，并将其赋值给task的submission属性。然后，根据task_submission的submit_type属性判断任务提交的类型，如果为"success"，则将tool_call的状态设置为ToolCallStatusCode.SUCCESS，否则设置为ToolCallStatusCode.FAILED。接着，将tool_call的输出设置为"Task submitted as {task_submission.submit_type}."。最后，将任务的状态、工具调用的状态和输出信息记录到日志中，并返回工具调用的状态和输出。

**注意**：使用此代码时需要注意以下几点：
- 该函数依赖于PlanNode、ToolCall和TaskSubmission对象。
- 需要确保传入的tool_call参数包含正确的参数，以便创建TaskSubmission对象。

**输出示例**：假设task_submission的submit_type为"success"，则返回的工具调用的状态为ToolCallStatusCode.SUCCESS，输出为"Task submitted as success."。
***
