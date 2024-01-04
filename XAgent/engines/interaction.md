# ClassDef InteractionMessages
**InteractionMessages 类的功能**：该类是用于管理交互消息的异步队列和相关的开关状态。

InteractionMessages 类定义了四个异步队列，分别用于存储中断（interruptions）、判断响应（judging_response）、路由响应（routing_response）和任务输入初始化（task_input_initial）的消息。这些队列在异步编程中用于跨不同协程之间传递消息，使得在处理用户交互或系统内部事件时能够更加灵活和响应式。

此外，该类还定义了四个布尔类型的开关状态，分别对应上述队列的启用状态。这些开关可以控制是否处理相应的消息队列中的消息，从而提供了一种动态调整系统行为的机制。

在项目中，InteractionMessages 类的实例被创建并用于 Interaction 引擎的初始化过程中。在 Interaction 引擎的构造函数中，通过创建一个 InteractionMessages 的实例来初始化与交互相关的消息队列和开关状态。这样，Interaction 引擎就可以在运行时根据需要启用或禁用特定的消息处理功能。

**注意**：
- InteractionMessages 类的实例应该在需要处理异步交互消息的上下文中创建和使用。
- 在使用异步队列时，需要注意协程的正确启动和关闭，以及消息的正确发送和接收，以避免潜在的竞态条件或死锁。
- 开关状态的改变应该谨慎进行，确保系统的行为符合预期，避免因不当的开关操作导致消息丢失或处理逻辑混乱。
- 由于涉及到异步编程，使用该类时需要有一定的 Python 异步编程知识，包括对 asyncio 库的理解和使用。
***
# ClassDef InteractionEngine
**InteractionEngine函数**: 这个类的功能是处理用户与系统之间的交互，负责接收用户的输入并进行相应的处理和响应。

该类继承自BaseEngine类，具有以下属性和方法：

- 属性：
  - plan_engine: PlanEngine对象，用于执行计划引擎的任务。
  - execution_engine: BaseEngine对象，用于执行任务的引擎。
  - task_formation_history: 保存任务形成的历史记录。
  - user_interaction_history: 保存用户与系统的交互历史记录。
  - task_running_status: 任务运行状态，0表示路由中，1表示下游引擎已启动，2表示引擎已停止。
  - current_task: 当前任务的描述。
  - model: 默认模型。
  - sys_prompt_route: 路由功能的系统提示。
  - functions_route: 路由功能的函数。
  - sys_prompt_judge: 判断功能的系统提示。
  - functions_judge: 判断功能的函数。
  - interaction_messages: InteractionMessages对象，用于处理中断消息。

- 方法：
  - \_\_init\_\_(self, config=CONFIG, plan_engine: PlanEngine = None, execution_engine: BaseEngine = None): 初始化方法，接收配置、计划引擎和执行引擎作为参数。
  - get_user_input(self, user_input): 连续提示用户输入，并将其放入中断队列中。
  - check_running_status(self): 持续检查任务是否完成的方法。
  - generate(self, messages, tools, tool_choice): 生成GPT响应的方法。
  - handle_interrupts(self): 处理用户中断的方法。
  - judging(self, function_call, interrupt_msg): 执行GPT判断的方法。
  - high_priority_interrupt(self, prompt: str): 高优先级输入回调的方法。
  - initial_task_formation(self): 与用户聊天并形成初始任务的方法。
  - run(self): 运行方法，是该类的入口点。

**注意**: 
- 该类用于处理用户与系统之间的交互，包括接收用户输入、生成响应、处理中断等功能。
- 通过调用不同的方法和属性，可以实现与用户的交互和任务执行的控制。
- 该类需要与其他引擎类（如PlanEngine和ReActEngine）配合使用，以实现完整的任务执行流程。

**输出示例**:
以下是InteractionEngine类的一个示例用法：

```python
from XAgent.config import CONFIG
from XAgent.engines import PlanEngine, ReActEngine, InteractionEngine

# 创建计划引擎和执行引擎
plan_engine = PlanEngine(config=CONFIG)
execution_engine = ReActEngine(config=CONFIG)

# 创建交互引擎，并传入计划引擎和执行引擎
interaction_engine = InteractionEngine(
    config=CONFIG,
    plan_engine=plan_engine,
    execution_engine=execution_engine,
)

# 运行交互引擎
await interaction_engine.run()
```

在上述示例中，我们首先创建了计划引擎和执行引擎，然后创建了交互引擎，并将计划引擎和执行引擎作为参数传递给交互引擎的构造函数。最后，我们调用交互引擎的run方法来运行交互引擎，实现用户与系统的交互和任务执行的控制。
## FunctionDef __init__
**__init__ 函数**: 该函数的作用是初始化交互引擎对象。

该函数是交互引擎类的构造函数，用于初始化交互引擎实例。构造函数接收三个参数：`config`、`plan_engine` 和 `execution_engine`。其中 `config` 参数提供了配置信息，默认值为 `CONFIG` 常量；`plan_engine` 参数是一个计划引擎实例，用于处理任务的规划；`execution_engine` 参数是一个执行引擎实例，用于执行具体的任务。

在函数体内，首先通过 `super().__init__(config)` 调用基类的构造函数，传入配置信息。

接下来，函数设置了几个实例变量：
- `self.plan_engine`：存储传入的计划引擎实例。
- `self.execution_engine`：存储传入的执行引擎实例。
- `self.task_formation_history`：初始化为空列表，用于记录任务形成的历史。
- `self.user_interaction_history`：初始化为空列表，用于记录用户交互的历史。
- `self.task_running_status`：初始化为0，表示任务的运行状态，其中0代表正在路由，1代表下游引擎已启动，2代表引擎已停止。
- `self.current_task`：用于测试，存储了一个字符串描述的任务。

此外，函数还初始化了与 XAgent 相关的一些变量：
- `self.model`：从配置中获取默认模型。
- `self.sys_prompt_route` 和 `self.functions_route`：通过 `function_manager.get_function_schema("Router")` 获取路由功能的提示和函数。
- `self.sys_prompt_judge` 和 `self.functions_judge`：通过 `function_manager.get_function_schema("Judger")` 获取判断功能的提示和函数。

最后，函数创建了一个 `InteractionMessages` 实例，存储在 `self.interaction_messages` 中，用于处理中断时的交互消息。

**注意**：
- 在使用该构造函数时，需要确保传入的 `config` 参数包含了所需的配置信息，特别是 `request.default_model`，因为它会被用来初始化模型。
- `plan_engine` 和 `execution_engine` 参数是可选的，如果不传入，则这些引擎实例将不会被初始化，需要在后续的代码中进行设置。
- `function_manager.get_function_schema` 函数用于获取特定功能的提示和函数定义，需要确保 `function_manager` 已经被正确初始化，并且包含了 "Router" 和 "Judger" 这两个功能的定义。
- `InteractionMessages` 类应该提供了处理交互消息所需的接口和功能，但在这段代码中没有给出具体实现，需要查看相关的类定义以了解详细信息。
## AsyncFunctionDef get_user_input
**get_user_input函数**: 该函数的作用是处理用户输入，并根据不同的交互消息状态将用户输入放入相应的队列中。

该`get_user_input`函数是一个异步函数，它接收一个参数`user_input`，这个参数代表用户的输入。函数内部根据`self.interaction_messages`对象的不同状态，将用户输入放入不同的队列中，以便后续处理。

具体来说，函数的处理逻辑如下：

1. 如果`self.interaction_messages.enable_task_input_initial`为真，表示当前处于初始任务形成阶段，用户的输入将被放入`task_input_initial`队列中，等待后续的任务处理。

2. 如果`self.interaction_messages.enable_routing_response`为真，表示计划引擎尚未启动，此时用户的输入将被用于路由响应，输入将被放入`routing_response`队列中。

3. 如果`self.interaction_messages.enable_judging_response`为真，表示当前支持内部对用户响应进行分类，用户的输入将被放入`judging_response`队列中。

4. 如果`self.interaction_messages.enable_interruptions`等于1，表示当前允许中断，用户的输入将被放入`interruptions`队列中。

在项目中，`get_user_input`函数被`XAgent/workflow/interaction_loop.py`文件中的`get_user_input`方法调用。在`interaction_loop.py`中，该方法作为交互循环的一部分，用于接收用户的消息，并通过调用`interact_engine`对象的`get_user_input`方法将消息传递给引擎处理。

**注意**：
- 由于`get_user_input`是一个异步函数，调用它时需要使用`await`关键字，以确保异步执行。
- 在使用该函数时，需要确保`self.interaction_messages`对象已经被正确初始化，并且其状态属性被正确设置。
- 该函数不返回任何值，它的主要作用是将用户输入放入正确的队列中，以便后续的处理流程可以继续执行。
## AsyncFunctionDef check_running_status
**check_running_status 函数**: 该函数的功能是监控任务运行状态，并在任务完成时更新状态并处理中断。

该`check_running_status`函数是一个异步函数，它的主要作用是在任务执行过程中周期性地检查任务的运行状态。函数内部实现了一个无限循环，每隔1秒钟就会执行一次循环体内的代码。

在循环体中，首先通过`asyncio.sleep(1)`实现了异步等待，不会阻塞其他并发任务的执行。接着，函数会检查`self.task_running_status`变量的值，该变量用于表示任务的运行状态。当状态值为1时，表示任务正在运行中。

如果任务正在运行（即`self.plan_task`异步任务尚未完成），则继续循环等待。如果`self.plan_task`已经完成，则会将`self.task_running_status`状态更新为2，表示任务已经完成。同时，将`self.interaction_messages.enable_interruptions`设置为`False`，禁用中断处理，并通过抛出`asyncio.CancelledError`异常来取消当前的异步任务。

在项目中，`check_running_status`函数被调用于`interaction.py`文件的`run`方法中。在`run`方法中，首先会启动任务的初始阶段，并创建任务执行的异步任务`self.plan_task`。然后，设置任务运行状态为1，并启用中断处理。接着，创建`self.status_checking_task`异步任务来执行`check_running_status`函数，以监控任务的运行状态。

在`run`方法的`try`块中，使用`asyncio.gather`等待`self.status_checking_task`和其他相关的异步任务。如果在执行过程中捕获到`asyncio.CancelledError`异常，表示`check_running_status`函数中的任务已经完成或被取消，此时会记录相应的日志信息。最后，在`finally`块中取消所有相关的异步任务，并记录任务完成的日志信息。

**注意**：
- 由于`check_running_status`函数内部包含无限循环，因此必须要有明确的退出条件，本例中是任务完成时通过抛出异常来退出循环。
- 在使用`asyncio.gather`等待多个异步任务时，如果其中一个任务因为异常而退出，其他任务也会被取消。
- 在设计异步函数时，需要注意异常处理和资源清理，确保异步任务能够正确地开始和结束。
## AsyncFunctionDef generate
**generate函数**: 该函数的功能是异步生成工具调用结果。

该`generate`函数是一个异步函数，它的主要作用是发送用户消息，并基于这些消息以及指定的工具和工具选择来生成工具调用的结果。函数接收三个参数：`messages`，`tools`，和`tool_choice`。

- `messages`参数是一个消息列表，通常包含系统提示和用户互动的历史信息。
- `tools`参数是一个工具列表，这里的工具指的是可以被调用来处理消息的函数或服务。
- `tool_choice`参数是一个字符串，表示选择使用哪个具体的工具或函数来处理消息。

函数内部，首先调用`objgenerator.chatcompletion`方法，该方法负责处理消息并生成工具调用结果。这个调用是异步的，因此在调用前使用了`await`关键字。`objgenerator.chatcompletion`方法返回一个元组，其中第二个元素是工具调用的结果列表。函数打印出结果列表中第一个元素的`arguments`属性，并将其作为返回值。

在项目中，`generate`函数被用于处理中断和初始化任务形成。例如，在处理中断时，它会生成一个判断中断是否有效的函数调用；在初始化任务形成时，它会与用户交互，以确定用户的任务需求，并生成相应的路由函数调用。

**注意**:
- 由于`generate`函数是异步的，调用它的代码需要在`async`函数中，并且使用`await`关键字。
- 函数调用`objgenerator.chatcompletion`可能会抛出异常，因此在实际使用时可能需要进行异常处理。
- 函数打印的`tool_call_res[0].arguments`是为了调试目的，实际部署时可能需要移除或修改这部分代码。

**输出示例**:
假设`tool_call_res[0].arguments`返回的是一个包含工具调用参数的字典，那么函数的返回值可能看起来像这样：
```python
{
    'tool_name': 'Judger',
    'arguments': {
        'input_text': '用户提出的中断消息',
        'other_param': '其他参数值'
    }
}
```
这个字典包含了用于后续处理的工具名称和参数。
## AsyncFunctionDef handle_interrupts
**handle_interrupts函数**: 该函数的功能是处理交互中的中断事件。

该函数是一个异步函数，它的主要作用是在XAgent的交互引擎中不断检查是否有中断消息，并对这些中断消息进行处理。函数的执行逻辑如下：

1. 函数通过一个无限循环，每隔1秒钟检查一次是否有新的中断消息。
2. 当检测到中断消息时，首先会判断当前任务是否正在运行（`self.task_running_status == 1`），只有在任务运行状态为1时，才会接受并处理中断。
3. 如果任务正在运行，函数会记录一条日志信息，表明开始进行中断判断。
4. 接下来，函数会构造一系列消息，这些消息包括系统提示、用户交互历史以及用户的中断消息。
5. 然后，函数会调用`self.generate`方法，生成一个函数调用请求，用于判断中断。
6. 最后，函数会调用`self.judging`方法，传入函数调用请求和中断消息，进行中断的判断处理。

在项目中，`handle_interrupts`函数被`interaction.py`文件中的`run`方法调用。在`run`方法中，首先会启动任务的初始构成，然后设置任务运行状态为1，并启用中断消息的接收。之后，会创建一个异步任务来执行`handle_interrupts`函数，同时还会创建其他相关的异步任务，如状态检查和计划引擎的中断监听。这些任务会一起运行，直到其中一个任务完成或被取消。如果出现取消错误，会记录相应的日志信息，并取消所有相关的异步任务。

**注意**：
- 由于`handle_interrupts`函数包含无限循环，因此在调用该函数时需要注意适当的退出条件，以避免造成无限等待。
- 函数内部使用了`asyncio.sleep`来进行异步等待，这意味着它不会阻塞其他并发运行的协程。
- 在处理中断时，函数依赖于其他方法（如`self.generate`和`self.judging`），因此在使用`handle_interrupts`函数之前，需要确保这些依赖方法已经正确实现。
- 该函数设计为协程（coroutine），因此在调用时需要使用`await`关键字或将其注册到事件循环中。
- 函数的执行依赖于`self.task_running_status`的状态，只有当状态为1时，函数才会处理中断。这意味着在任务未运行或已完成时，中断不会被处理。
## AsyncFunctionDef judging
**judging函数**: 该函数的功能是根据用户中断消息和功能调用信息来判断并执行相应的操作。

judging函数是一个异步函数，它的主要作用是处理用户的中断请求。当用户在执行任务过程中发出中断请求时，该函数会根据提供的功能调用信息来判断用户的意图，并执行相应的操作。

函数接收两个参数：`function_call`和`interrupt_msg`。`function_call`是一个字典，包含了用户意图的判断结果和相关参数；`interrupt_msg`是用户的中断消息。

函数内部首先记录用户的中断消息到用户交互历史中。然后根据`function_call`中的`judgement`字段来执行不同的操作：

- 如果判断结果是`InterationJudge.exit`，即用户想要退出当前任务，函数会询问用户是否真的想要退出，并等待用户的响应。如果用户响应为"Y"，则取消当前的任务并抛出`asyncio.CancelledError`异常；如果用户响应不是"Y"，则继续执行任务。
- 如果判断结果是`InterationJudge.respond`，即用户想要得到某种响应，函数会记录下用户的请求参数。
- 如果判断结果是`InterationJudge.chat`，即用户想要进行聊天，函数会根据用户的请求参数决定使用计划引擎还是执行引擎来处理聊天，并等待聊天引擎的响应。
- 如果判断结果是`InterationJudge.detail`，即用户想要详细信息，函数会记录用户的附加指令，并设置中断信息。

如果`judgement`字段的值不是上述任何一个预定义的选项，函数会抛出异常，表示判断结果无效。

**注意**：
- 该函数是异步的，调用时需要使用`await`。
- 函数中使用了`asyncio.sleep`来暂停执行，这可能是为了等待某些异步操作的完成。
- 函数中的`plan_task.cancel()`用于取消当前的任务，这需要在外部定义`plan_task`。
- 函数中的`set_interrupt`和`set_interrupt_message`用于设置中断信息，这些也需要在外部定义。
- 函数中使用了`recorder.log`来记录日志，这需要在外部定义`recorder`对象，并且确保它有`log`方法。
- 函数中的`InterationJudge`应该是一个枚举类型，包含了不同的判断结果选项。

**输出示例**：
由于该函数没有返回值，它主要通过抛出异常或者记录日志来表达执行结果。因此，没有具体的返回值示例。但是，函数执行的结果会影响到用户交互历史的记录，以及可能导致当前任务的取消或继续执行。
## AsyncFunctionDef routing
**routing函数**: 该函数的功能是根据函数调用的参数来决定如何路由交互，并返回是否结束当前交互循环以及是否启动下游引擎的指示。

该`routing`函数是一个异步函数，它处理基于用户输入和系统提示的交互路由。它接收一个名为`function_call`的参数，这是一个字典，包含了交互过程中需要的信息，如`argument`、`route_to`和`restart`。

函数内部首先解析`function_call`字典中的参数。`argument`是与用户交互的信息内容，`route_to`是一个枚举值，指示消息应该路由到哪个处理流程，`restart`是一个字符串，表示是否需要重新启动交互循环。

函数中使用了Python 3.10引入的结构化模式匹配（match-case语句），根据`route_to`的值来决定不同的处理流程：

- `InterationRoute.GPT`：如果路由到GPT，函数会记录直接来自GPT的回复，并询问用户是否需要输入响应。如果不需要重新启动（restart为False），则会等待用户的输入，并将用户和助手的消息添加到任务形成历史中。函数返回是否重新启动和下游引擎是否应该启动的布尔值。

- `InterationRoute.User`：如果路由到用户，函数会请求用户提供更多信息，并等待用户的输入。用户的输入会被添加到任务形成历史中，函数返回两个False，表示不结束当前循环，也不启动下游引擎。

- `InterationRoute.Downstream`：如果路由到下游，函数会记录任务目标的总结，并告知用户将开始执行任务。函数返回True，表示结束当前循环并启动下游引擎。

- 默认情况（case _）：如果`route_to`的值不是预期的枚举值，函数会抛出异常。

在项目中，`routing`函数被`initial_task_formation`函数调用。在`initial_task_formation`函数中，它用于处理用户输入的任务，并通过与用户的交互来形成任务的最终形态。一旦任务形成完成，或者用户决定开始执行任务，`routing`函数将指示是否结束交互循环并启动下游引擎。

**注意**：使用`routing`函数时，需要确保传入的`function_call`字典格式正确，并且包含了必要的键和值。此外，由于使用了异步编程模式，调用此函数时需要使用`await`关键字，并且调用它的环境也应该是异步的。

**输出示例**：
```python
# 假设function_call字典如下：
function_call = {
    "argument": "需要处理的任务描述",
    "route_to": InterationRoute.User,
    "restart": "no"
}

# 调用routing函数可能返回的结果：
(False, False)  # 表示不结束当前循环，也不启动下游引擎
```

以上就是对`routing`函数的详细说明文档。
## AsyncFunctionDef high_priority_interrupt
**high_priority_interrupt 函数**: 此函数的功能是处理高优先级的中断。

详细代码分析与描述:
`high_priority_interrupt` 是一个异步函数，它是定义在某个类中的一个方法。这个函数接收一个参数 `prompt`，这个参数是一个字符串类型，用于传递触发高优先级中断的提示信息或者命令。

由于函数体中只有一个 `pass` 语句，这表明当前函数的实现尚未完成。在实际的应用中，这个函数可能会被设计为处理那些需要立即响应的情况，比如紧急事件或者特定的用户指令，它将中断当前的处理流程，优先处理这个高优先级的任务。

在异步编程中，`async` 关键字表示该函数是一个协程（coroutine），它可以在执行过程中暂停和恢复，这使得它可以在等待某些操作完成（如I/O操作）时，让出控制权，以便其他协程可以运行。

由于这个函数目前没有实现具体的功能，所以在实际使用时，开发者需要根据具体的应用场景来填充函数体，实现高优先级中断的具体逻辑。

**注意**:
- 在使用这个函数之前，需要确保它已经被正确地实现了具体的逻辑。
- 由于这是一个异步函数，调用它时需要使用 `await` 关键字，或者在其他异步函数中调用它。
- 开发者在实现函数时，应当考虑到中断处理可能会影响到程序的正常流程，因此在设计中断逻辑时要特别小心，确保程序的稳定性和数据的一致性。
## AsyncFunctionDef initial_task_formation
**initial_task_formation函数**: 此函数的功能是初始化任务形成过程。

该函数`initial_task_formation`是一个异步函数，它负责启动与用户的交互，收集用户输入的任务，并开始任务路由过程。以下是对代码的详细分析：

1. 首先，函数创建了一个任务形成历史记录列表`self.task_formation_history`，并将系统提示消息作为第一条消息添加到该列表中。

2. 然后，函数通过记录器输出绿色的提示信息，要求用户输入任务。

3. 接下来，函数设置`self.interaction_messages.enable_task_input_initial`为True，允许用户输入任务，并等待用户的输入。用户输入后，将其添加到任务形成历史记录列表中，并关闭任务输入功能。

4. 函数进入一个循环，通过调用`self.generate`函数生成与用户的交互消息，并通过`self.routing`函数处理这些消息。如果`self.routing`函数返回表示完成的信号，循环将终止。

5. 循环结束后，函数将一条消息添加到用户交互历史记录中，表明用户已经提供了任务，下游的计划和执行引擎已经启动，并且任务已经开始运行。

6. 最后，函数返回`start_downstream`变量，表示是否开始下游任务。

**调用情况**:
`initial_task_formation`函数在`interaction.py`文件的`run`方法中被调用。在`run`方法中，首先调用`initial_task_formation`函数来初始化任务形成过程。如果`start_downstream`为False，则表示当前任务轮次结束。如果为True，则继续执行后续的计划引擎和执行引擎，并处理中断。

**注意**:
- 由于`initial_task_formation`是一个异步函数，调用它时需要使用`await`关键字。
- 在使用该函数时，确保`self.interaction_messages`和`self.functions_route`等相关属性已经被正确初始化。
- 函数内部的循环可能会因为用户输入或内部逻辑而持续一段时间，因此在设计交互逻辑时要考虑到用户体验。

**输出示例**:
由于`initial_task_formation`函数的返回值`start_downstream`依赖于用户输入和内部路由逻辑，因此无法提供一个固定的输出示例。不过，它通常会返回一个布尔值，表示是否开始下游任务的执行。
## AsyncFunctionDef run
**run函数**: 该函数的功能是执行交互式任务的整个流程。

该`run`函数是一个异步函数，它负责启动和管理一个交互式任务的执行过程。以下是详细的代码分析：

1. 首先，函数通过`recorder.log`记录了一条绿色的日志信息，表示交互任务的开始。

2. 接着，函数调用`self.initial_task_formation`方法来初始化任务。如果该方法返回`False`，表示没有任务需要执行，函数将记录一条日志并返回。

3. 如果有任务需要执行，函数将通过`recorder.save_query`保存当前任务的查询信息。

4. 然后，函数创建了一个`PlanNode`实例`inital_node`，这个节点代表了任务的起始点，并设置了角色和目标。

5. 接下来，函数使用`asyncio.create_task`创建了一个异步任务`self.plan_task`，该任务负责运行规划引擎`self.plan_engine`。

6. 函数设置了`self.task_running_status`为1，表示任务开始运行，并允许中断。

7. 函数创建了三个异步任务来处理中断：`self.interrupt_handler`处理中断事件，`self.status_checking_task`检查任务运行状态，`self.plan_interrupt_listener`监听规划引擎的中断事件。

8. 使用`asyncio.gather`等待上述创建的异步任务完成。如果在等待过程中发生了`asyncio.CancelledError`异常，表示规划引擎的执行被取消了，函数将记录一条日志信息。

9. 最后，无论任务是否正常完成，函数都会尝试取消这些异步任务，并记录任务结束的日志信息。

**注意**：
- 由于`run`函数是异步的，调用它时需要使用`await`关键字。
- 在使用该函数之前，确保相关的类和方法已经正确初始化，并且异步环境已经设置好。
- 函数中使用了`asyncio.create_task`来创建异步任务，这要求Python版本至少为3.7。

**输出示例**：由于该函数没有返回值，它的主要作用是执行一系列的异步操作，因此没有具体的返回值示例。不过，函数执行过程中会有日志输出，例如：
```
=-=-=-=-=-=-=- Interaction Begin =-=-=-=-=-=-=-
=== Finish Current Round of Task ===
Plan engine execution cancelled.
=== Finish Current Round of Task ===
```
***
