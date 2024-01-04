# ClassDef BaseEngine
**BaseEngine函数**: 这个类的功能是XAgent的基础引擎，负责执行工作流程并确定工具的执行顺序。

该类具有以下方法：

- `__init__(self, config: XAgentConfig = CONFIG, toolifs: list[BaseToolInterface] = [], agent: BaseAgent = None)`: 初始化方法，用于设置配置、工具接口和代理对象等属性。

- `chat(self, usr_query: str, **kwargs) -> str`: 与用户进行对话的方法，接收用户的查询并返回回复。

- `get_file_system_structure(self) -> str`: 获取文件系统结构的方法，返回文件系统的结构信息。

- `lazy_init(self, config=None, get_available_tools=True)`: 惰性初始化方法，用于延迟初始化配置和获取可用工具。

- `get_available_tools(self) -> Tuple[list[str], list[dict]]`: 获取可用工具的方法，返回可用工具的名称和工具的描述信息。

- `retrieve_tools(self, query: str, top_k: int = None) -> Tuple[list[str], list[dict]]`: 检索工具的方法，根据查询返回匹配的工具名称和工具的描述信息。

- `execute(self, tool_call: ToolCall) -> Tuple[ToolCallStatusCode, Any]`: 执行工具调用的方法，根据工具调用执行相应的操作。

- `step(self, **kwargs) -> ExecutionNode`: 执行步骤并返回执行结果的方法。

- `run(self, task: PlanNode, **kwargs) -> ExecutionGraph`: 执行引擎并返回结果节点的方法。

注意事项：
- 在使用该类的方法时，需要根据具体的需求和参数进行调用。
- 该类的方法可以根据具体的业务逻辑进行扩展和定制。
- 请确保在调用方法之前已经进行了适当的初始化和配置。

示例输出：
```python
from XAgent.engines import BaseEngine

# 创建BaseEngine对象
engine = BaseEngine()

# 调用chat方法进行对话
response = engine.chat("你好")

# 输出回复
print(response)
```

**注意**: 以上示例仅为演示目的，实际使用时需要根据具体情况进行调整。
## FunctionDef __init__
**__init__ 函数**: 该函数的作用是初始化 Engine 类的实例。

该函数是 Engine 类的构造函数，用于初始化 Engine 类的实例。它接受三个参数：config、toolifs 和 agent。以下是对每个参数的详细分析：

- `config`: 类型为 XAgentConfig，默认值为 CONFIG。这个参数用于提供引擎配置信息，包括但不限于工具服务器的地址、认证信息等。

- `toolifs`: 类型为 BaseToolInterface 的列表，默认为空列表。这个参数用于传入一系列的工具接口实例，这些工具接口定义了与特定工具交互的方法。

- `agent`: 类型为 BaseAgent，默认为 None。这个参数允许传入一个代理实例，代理可以用来执行特定的任务或者管理工具的执行。

在函数体内部，首先将传入的参数赋值给实例变量。然后，初始化了几个额外的实例变量：

- `toolif_mapping`: 一个字典，用于存储工具接口名称到工具接口实例的映射。

- `available_tools`: 一个字符串列表，用于存储可用工具的名称。

- `tools_schema`: 一个字典列表，用于存储工具的模式信息。

- `interrupt_response_schema`: 通过调用 function_manager.get_function_schema 方法获取中断响应的模式。

- `exec_track`: 类型为 ExecutionGraph，用于存储执行图，它记录了任务执行的流程和状态。

- `exec_node`: 类型为 ExecutionNode，用于存储当前的执行节点信息。

- `interrupt`: 调用 register_new_interrupt_vector 函数注册一个新的中断向量，用于处理中断情况。

**注意**：
- 在使用该构造函数时，需要确保传入的 config 参数符合 XAgentConfig 类型的要求。
- 如果提供 toolifs 参数，需要确保列表中的每个元素都是 BaseToolInterface 类型的实例。
- agent 参数是可选的，如果提供，需要是 BaseAgent 类型的实例。
- 在实例化 Engine 类之后，可能需要根据实际情况进一步配置 toolif_mapping、available_tools 等属性，以确保引擎能够正常工作。
## FunctionDef __del__
**__del__ 函数**: 该函数的功能是在对象被销毁时注销中断向量。

当一个对象的生命周期结束时，Python 的垃圾回收机制会调用该对象的 `__del__` 方法。在这个 `__del__` 方法中，它会检查对象是否有一个名为 `interrupt` 的属性，如果这个属性不是 `None`，那么它会调用 `unregister_interrupt_vector` 函数来注销与该 `interrupt` 相关联的中断向量。

具体来说，`self.interrupt` 可能是一个表示中断向量的对象或者是一个标识符，它在对象创建时可能被注册以便在特定事件发生时触发某些操作。当对象不再需要，即将被销毁时，我们需要确保这些注册的中断向量也被相应地清理掉，以避免潜在的资源泄露或者不必要的触发。

这里的 `unregister_interrupt_vector` 函数应该是一个在其他地方定义的函数，它的职责是接受一个中断向量作为参数，并将其从中断向量的注册表中移除。

**注意**：
- 在使用 `__del__` 方法时，需要注意不要在该方法中引用可能会导致循环引用的对象，因为这可能会干扰 Python 的垃圾回收机制。
- `__del__` 方法的调用时机不是由程序员直接控制的，而是由 Python 解释器在对象生命周期结束时自动调用的，因此可能不会立即执行。
- 如果 `unregister_interrupt_vector` 函数依赖于全局变量或者其他模块的状态，那么在程序关闭时（解释器进程结束时），这些依赖可能已经不在有效状态，这时在 `__del__` 方法中调用 `unregister_interrupt_vector` 可能会引发异常。因此，通常建议在程序的正常运行阶段显式地清理资源，而不是依赖 `__del__` 方法的隐式调用。
## AsyncFunctionDef chat
**chat函数**: 该函数的功能是处理用户的聊天请求，并返回响应。

该`chat`函数是一个异步函数，定义在XAgent项目的`engines/base.py`文件中。它的主要作用是接收用户的查询（usr_query），并通过与代理（agent）交互来获取回复。该函数接受一个字符串类型的用户查询，并返回一个字符串类型的响应。

函数的详细流程如下：
1. 函数首先调用`self.agent.step`方法，传入以下参数：
   - `step_arguments`: 通过`**kwargs`传入的额外参数。
   - `message`: 一个`Message`对象，代表用户的角色和内容。
   - `replies`: 通过`function_manager.get_function_schema("chat")`获取的聊天功能参数。
   - `tools`: 一个空列表，表示在这一步骤中不使用任何工具。
   - `setup_summary_tasks`: 一个布尔值，表示是否设置总结任务。
   - `update_working_context`: 一个布尔值，表示是否更新工作上下文。
   - `chat_mode`: 一个布尔值，表示是否处于聊天模式。
2. `self.agent.step`方法返回两个值：`replies`和`tool_calls`。在这个函数中，只使用了`replies`。
3. `replies`是一个列表，但这里只取第一个元素。
4. 从`replies`中取出的回复内容被添加到响应字符串`response`中。
5. 函数返回最终的响应字符串。

在项目中，`chat`函数被`XAgent/engines/interaction.py`文件中的`judging`函数调用。在`judging`函数中，根据用户的判断结果（judgement），可能会调用`chat`函数来处理用户的聊天请求。如果判断结果是`InterationJudge.chat`，则会创建一个聊天引擎实例（chat_engine），并调用其`chat`方法，传入用户的消息（interrupt_msg）和计划概览（plan_overview）作为参数。

**注意**：
- 由于`chat`函数是异步的，调用它时需要使用`await`关键字。
- 在调用`chat`函数时，可以传入额外的关键字参数，这些参数将会影响`self.agent.step`方法的行为。
- `chat`函数返回的是一个字符串，代表聊天的响应内容。

**输出示例**：
假设用户的查询是"你好，我想了解更多关于XAgent的信息。"，`chat`函数可能返回的响应字符串为："你好！XAgent是一个智能代理，它可以帮助你自动化执行各种任务。"
## AsyncFunctionDef get_file_system_structure
**get_file_system_structure函数**：该函数的作用是获取文件系统的结构。

该函数首先导入了`XAgent.tools.ToolServerInterface`模块，并调用其`execute`方法来执行`print_filesys_struture`命令，获取文件系统的结构。然后，对获取到的文件系统结构进行了处理，将其转换为字符串，并进行了截断处理，最多保留1000个字符。如果获取文件系统结构的过程中出现异常，则会记录警告信息，并打印异常堆栈信息。最后，将处理后的文件系统结构返回。

**注意**：在使用该函数时需要注意以下几点：
- 需要导入`XAgent.tools.ToolServerInterface`模块。
- 需要保证`ToolServerInterface`的`execute`方法能够执行`print_filesys_struture`命令。

**输出示例**：假设获取到的文件系统结构为：
```
/
├── home
│   ├── user
│   │   ├── file1.txt
│   │   └── file2.txt
│   └── admin
│       ├── file3.txt
│       └── file4.txt
└── var
    ├── log
    │   └── app.log
    └── tmp
        └── temp.txt
```

则函数返回的文件系统结构字符串为：
```
/
├── home
│   ├── user
│   │   ├── file1.txt
│   │   └── file2.txt
│   └── admin
│       ├── file3.txt
│       └── file4.txt
└── var
    ├── log
    │   └── app.log
    └── tmp
        └── temp.txt
```
## AsyncFunctionDef lazy_init
**lazy_init函数**: 这个函数的作用是在引擎初始化时进行懒加载，根据传入的配置参数进行初始化设置，并获取可用的工具。

在代码中，lazy_init函数是一个异步函数，它接受一个config参数和一个get_available_tools参数。如果传入了config参数，则将其赋值给self.config。如果self.agent不为空，则调用self.agent的lazy_init方法，并传入self.config作为参数。如果get_available_tools为True，则调用self.get_available_tools方法。

在项目中，lazy_init函数被以下文件调用：
- 文件路径：XAgent/engines/base.py
  对应代码如下：
    async def run(self,
                  task: PlanNode,
                  **kwargs) -> ExecutionGraph:
        """执行引擎并返回结果节点。"""
        await self.lazy_init(self.config)
        raise NotImplementedError

- 文件路径：XAgent/engines/pipeline.py
  对应代码如下：
    async def run(self, task: PipelineTaskNode, **kwargs) -> ExecutionGraph:
        """传入pipeline文件，执行pipeline"""
        await self.lazy_init(self.config)
        ...

- 文件路径：XAgent/engines/plan.py
  对应代码如下：
    async def run(self,
                  task: PlanNode,
                  execution_engine: BaseEngine,
                  **kwargs) -> PlanExecutionGraph:
        """执行引擎并返回结果节点。"""
        await self.lazy_init(self.config, get_available_tools=False)
        ...

- 文件路径：XAgent/engines/react.py
  对应代码如下：
    async def run(self,
                  task: PlanNode,
                  plan_tree: PlanTree,
                  plan_interruption_vec: int,
                  **kwargs) -> ReActExecutionGraph:
        """执行引擎并返回结果节点。"""
        await self.lazy_init(self.config)
        ...

- 文件路径：sample/interrupt_1.py
  对应代码如下：
        async def run(self):
            await self.lazy_init(self.config)
            ...

- 文件路径：sample/interrupt_2.py
  对应代码如下：
        async def run(self):
            await self.lazy_init(self.config)
            ...

**注意**: 在使用lazy_init函数时需要注意以下几点：
- 可以传入config参数来定制引擎的配置。
- 如果self.agent不为空，则会调用self.agent的lazy_init方法进行初始化。
- 如果get_available_tools为True，则会调用self.get_available_tools方法获取可用的工具。
## AsyncFunctionDef get_available_tools
**get_available_tools函数**: 该函数的功能是获取可用的工具列表和工具模式(schema)。

该`get_available_tools`函数是一个异步函数，它的主要作用是从系统中注册的工具接口(`toolifs`)中获取所有可用的工具名称和对应的工具模式。函数返回两个列表：一个包含字符串的列表，代表所有可用工具的名称；另一个包含字典的列表，代表每个工具的模式(schema)。

具体来说，函数首先初始化两个空列表`available_tools`和`tools_schema`，以及一个空字典`toolif_mapping`。然后，函数遍历所有注册的工具接口(`toolifs`)，对每个接口执行`lazy_init`方法进行初始化，并调用接口的`get_available_tools`方法获取该接口下的工具列表和工具模式。随后，函数将获取到的工具名称添加到`available_tools`列表中，并更新`toolif_mapping`字典，将工具名称与其对应的接口关联起来。对于获取到的工具模式，函数将其封装为一个字典，其中包含类型（"function"）和工具模式本身，然后添加到`tools_schema`列表中。

在项目中，`get_available_tools`函数被调用的情况出现在`XAgent/engines/base.py`文件的`lazy_init`方法中。在`lazy_init`方法中，如果传入的参数`get_available_tools`为真（默认情况下是真），则会调用`get_available_tools`函数来初始化可用工具列表和工具模式。这通常在引擎的懒加载初始化过程中进行，以确保在引擎开始执行任务之前，所有可用的工具都已经被识别和准备好。

**注意**：
- 由于`get_available_tools`是一个异步函数，调用它时需要使用`await`关键字。
- 函数返回的两个列表中，`available_tools`包含的是字符串类型的工具名称，`tools_schema`包含的是字典类型的工具模式，这些模式可以用于后续的工具调用和处理。

**输出示例**：
```python
# 假设系统中注册了两个工具接口，每个接口提供了不同的工具和模式
available_tools = ['tool1', 'tool2']
tools_schema = [
    {"type": "function", "function": {"name": "tool1", "description": "Tool 1 description"}},
    {"type": "function", "function": {"name": "tool2", "description": "Tool 2 description"}}
]
```

在上述示例中，`available_tools`列表包含了两个工具名称"tool1"和"tool2"，而`tools_schema`列表则包含了这两个工具的模式，每个模式都是一个字典，包含了工具的类型和描述等信息。
## AsyncFunctionDef retrieve_tools
**retrieve_tools函数**: 该函数的功能是检索与给定查询字符串匹配的工具。

retrieve_tools函数是一个异步函数，它接收一个查询字符串`query`和一个可选的整数`top_k`，用于指定返回的最大工具数量。该函数的目的是从多个工具接口中检索与查询字符串匹配的工具，并将这些工具及其相关的JSON模式作为列表返回。

函数首先定义了两个空列表`available_tools`和`tools_schema`，用于存储检索到的工具名称和工具的JSON模式。如果参数`top_k`没有被指定，函数将使用配置中指定的最大检索数量`self.config.execution.max_tool_retrieve_count`作为默认值。

函数遍历`self.toolifs`中的每个工具接口，并调用接口的`retrieve_tools`方法，传入查询字符串`query`和数量限制`top_k`。每个接口返回的工具名称和JSON模式被添加到之前定义的列表中。同时，函数更新`self.toolif_mapping`字典，将每个工具名称映射到其对应的接口。

最后，函数返回两个列表：`available_tools`包含所有检索到的工具名称，`tools_schema`包含这些工具的JSON模式。

在项目中，`retrieve_tools`函数被`react.py`文件中的`run`方法调用。在`run`方法中，首先对引擎进行初始化，然后创建执行跟踪图`self.exec_track`。如果配置中指定了最大检索数量大于0，则调用`retrieve_tools`函数检索工具。检索到的工具模式被添加到代理的长期上下文中，用于后续的任务执行和决策。

**注意**:
- 由于`retrieve_tools`是一个异步函数，调用它时需要使用`await`关键字。
- 函数返回的是一个元组，包含两个列表，因此在接收返回值时需要解构赋值。
- 在调用此函数之前，确保`self.toolifs`已经被正确初始化，并且包含了所有可用的工具接口。

**输出示例**:
```python
(['tool1', 'tool2'], [{'type': 'tool', 'name': 'tool1', 'description': 'A tool for ...'}, {'type': 'tool', 'name': 'tool2', 'description': 'Another tool for ...'}])
```
在这个示例中，`available_tools`列表包含了两个检索到的工具名称`tool1`和`tool2`，`tools_schema`列表包含了这些工具的JSON模式，每个模式包括工具的类型、名称和描述。
## AsyncFunctionDef execute
**execute函数**：这个函数的作用是执行一个工具调用。

该函数接受一个ToolCall对象作为参数，其中包含了工具调用的名称和参数。函数首先会检查工具调用的名称是否为中断响应的名称，如果是的话，会返回中断状态码和相应的输出信息。然后，函数会记录工具调用的相关信息，并根据缓存判断是否需要重新执行工具调用。如果需要重新执行，函数会调用相应的接口执行工具调用，并等待工具调用和中断任务的完成。最后，函数会将执行结果更新到工具调用对象中，并将工具调用对象记录到记录器中。

在函数的调用部分，该函数在XAgent/engines/pipeline.py文件的run_action函数中被调用。在该函数中，根据不同的action类型，如果是ToolServer、N8N或Custom类型的action，会创建一个ToolCall对象，并调用execute函数执行工具调用。

在函数的另一个调用部分，该函数在XAgent/engines/react.py文件的step函数中被调用。在该函数中，根据中断状态和工具选择，会调用execute函数执行工具调用。

**注意**：在执行过程中，函数会根据中断状态和缓存判断是否需要重新执行工具调用，并将执行结果更新到工具调用对象中。

**输出示例**：假设工具调用的名称为"tool1"，参数为{"param1": "value1"}，执行结果为(ToolCallStatusCode.SUCCESS, "执行成功")。
## AsyncFunctionDef step
**step函数**: 此函数的功能是执行一步操作并返回执行结果。

`step` 函数是一个异步方法，它定义在一个类中，这个类可能代表了某种执行引擎或者处理流程的节点。由于这个函数体内部直接抛出了 `NotImplementedError` 异常，这表明 `step` 函数是一个抽象方法，需要在子类中被具体实现。

在具体的实现中，`step` 函数将会执行一些操作，并且返回一个 `ExecutionNode` 对象，这个对象可能包含了执行的结果或者状态信息。由于这是一个异步函数，它可以在协程中被调用，并且使用 `await` 关键字来等待其执行完成。

由于函数使用了 `**kwargs` 作为参数，这意味着它可以接受任意数量的关键字参数。这为函数的实现提供了很大的灵活性，因为在不同的子类中，可以根据需要接受不同的参数来执行相应的操作。

**注意**：
- 在使用此函数时，需要注意它必须在一个继承了该函数的子类中被具体实现，直接调用将会引发 `NotImplementedError` 异常。
- 由于这是一个异步函数，调用时需要使用 `await` 关键字。
- 函数的返回类型是 `ExecutionNode`，这意味着返回值应该是此类型的实例，具体的结构和内容取决于实现的逻辑。

**输出示例**：
由于 `step` 函数在这里是抽象的，没有具体的实现，因此无法提供一个具体的返回值示例。但是，一旦在子类中实现，它可能会返回类似于以下的 `ExecutionNode` 对象：

```python
# 假设的 ExecutionNode 实例
execution_node = ExecutionNode(
    status="success",
    result="操作结果",
    additional_info={"key": "value"}
)
```

在这个示例中，`ExecutionNode` 对象包含了执行状态、结果以及一些额外的信息。实际的返回值将取决于 `step` 函数在子类中的具体实现。
## AsyncFunctionDef run
**run函数**：该函数的功能是执行引擎并返回结果节点。

在代码中，run函数被调用的地方如下所示：

文件路径：XAgent/engines/plan.py
对应代码如下：
```python
async def step(self,
               handling_task: PlanNode,
               execution_engine: BaseEngine,
               **kwargs) -> PlanExecutionNode:
    """Step and return execution result."""
    handling_task.status = TaskStatusCode.DOING

    recorder.log(
        f"-=-=-=-=-=-=-= Performing Task {handling_task.pid} ({handling_task.goal}): Begin -=-=-=-=-=-=-=",
        Fore.GREEN,
    )
    

    # Start listening to interruptions in the background
    self.handling_task = handling_task
    self.during_exec = True
    
    execution_engine.general_query = self.general_query # for self evolving
    if isinstance(execution_engine, PipelineEngine):
        pipeline_dir = None
        candidates = await retrieve_pipeline(handling_task, vector_db, namespace=self.config.execution.evolve.inner_namespace)
        for candidate in candidates:
            retrieved_pipeline = candidate["value_sentence"]
            score = candidate["score"]
            # if retrieves non exist pipeline name, skip to next candidate
            if not os.path.exists(os.path.join(self.config.execution.pipeline.data_root, retrieved_pipeline, "decompiled_python_DSL_for_runner.py")):
                continue
            pipeline_dir = os.path.join(self.config.execution.pipeline.data_root, retrieved_pipeline)
            break
        
        exec_result = await execution_engine.run(
            task = PipelineTaskNode(
                pipeline_dir=pipeline_dir,
                overall_task_description=handling_task.goal,
                pipeline_input_params={}
            ),
            **kwargs
        )
            
    else:
      exec_result = await execution_engine.run(
          task=handling_task,
          plan_tree=self.exec_track.plan_tree,
          # Newly added, to enable set plan interruption in the react execution
          plan_interruption_vec=self.interrupt.vector,
          **kwargs)

    actions = []
    for node in exec_result.get_execution_track():
        actions.extend(node.tool_calls)

    self.exec_node = PlanExecutionNode(
        actions=actions,
        **handling_task.dump_task(),
    )
    handling_task.status = TaskStatusCode.SUCCESS if exec_result.status == EngineExecutionStatusCode.SUCCESS else TaskStatusCode.FAIL

    recorder.log(
        f"-=-=-=-=-=-=-= Task {handling_task.pid} ({handling_task.goal}): {handling_task.status.value} -=-=-=-=-=-=-=",
        handling_task.status.color(),
    )
    
    # waiting for the interruption handling to stop to ensuring rectify one at a time
    while self.interrupt_during_exec:
        await asyncio.sleep(0.5)
    # Stop listening to interruptions when the execution is done
    self.during_exec = False

    # Incorporate all the interruptions from buffer finally to th node
    if len(self.interrupt_msg_buffers) != 0:
        self.exec_node.interrupts.extend(self.interrupt_msg_buffers)
        self.interrupt_msg_buffers = []
    
    # We only handle the true plan rectify here. All the interruption handle go to the listener
    if getattr(exec_result, 'need_plan_rectification', False):
        await self.plan_rectify(
            handling_task=handling_task,
        )

    remaning_task: list[PlanNode] = self.exec_track.plan_tree.get_remining()
    self.exec_node.end_node = len(remaning_task) == 0

    return self.exec_node
```
（代码解析和描述...）
**注意**：关于代码使用的注意事项
**输出示例**：模拟代码返回值的可能外观。

请注意：
- 生成的内容中不应包含Markdown的标题和分隔符语法。
- 主要使用中文编写。如果需要，可以在分析和描述中使用一些英文单词，以提高文档的可读性，因为不需要将函数名或变量名翻译成目标语言。
***
