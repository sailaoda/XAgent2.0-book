# ClassDef BaseAgent
**BaseAgent函数**: 这个类的功能是作为一个基础代理类，提供了一些常用的方法和属性，用于处理代理的上下文、工具调用和对话等。

该类的构造函数`__init__`接受以下参数：
- `config: XAgentConfig`：XAgent的配置对象。
- `profile: str`：代理的简介信息。
- `step_prompt: str`（可选）：下一步的提示信息，默认为"What's next?"。
- `chat_prompt: str`（可选）：聊天时的提示信息，默认为"Answer the user's query based on the history."。
- `conclusion_prompt: str`（可选）：任务结束时的提示信息，默认为CONCLUSION。
- `default_replies: Literal["action_reasoning", "simple_thought", "observation_thought"]`（可选）：默认的回复类型，默认为"simple_thought"。

该类的属性包括：
- `config: XAgentConfig`：XAgent的配置对象。
- `profile`：代理的简介信息。
- `step_prompt`：下一步的提示信息。
- `chat_prompt`：聊天时的提示信息。
- `conclusion_prompt`：任务结束时的提示信息。
- `longterm_context`：长期上下文对象。
- `working_context: WorkingContext`：当前工作上下文对象。
- `default_replies`：默认的回复类型。
- `history_working_context`：历史工作上下文列表。
- `history_tool_calls`：历史工具调用字典。
- `_tool_call_summary_task`：工具调用摘要任务。
- `_context_summary_task`：上下文摘要任务。

该类提供了以下方法：
- `__str__()`：返回类的名称。
- `lazy_init(config: XAgentConfig)`：延迟初始化方法，用于更新配置对象。
- `context() -> list[str]`：返回代理的完整上下文。
- `get_step_message(mode: Literal["default", "conclusion"] = "default", **kwargs) -> str`：根据模式返回下一步的提示信息。
- `get_chat_message(**kwargs) -> str`：返回聊天时的提示信息。
- `get_task_conclusion(task: str, tasks_overview="`wrapped`") -> TaskConclusion`：返回任务的结论。
- `_setup_summary_tasks()`：设置摘要任务。
- `get_history_message(max_tokens) -> list[dict[str, str]]`：返回历史消息和过去的工作记忆。
- `step(step_arguments: dict = {}, message: Message = None, replies: dict = None, tools: list[dict] = None, tool_choice: dict = None, *, setup_summary_tasks: bool = True, update_working_context: bool = True, chat_mode=False, **kwargs) -> Tuple[list[dict], list[ToolCall]]`：进行下一步的思考/行动，并返回回复和工具调用。
- `_get_basic_info() -> dict[str]`：获取基本信息。
- `update_tool_calls_output(tool_calls: list[ToolCall]) -> str`：更新工具调用的输出。
- `update_longterm_context(user_message: str = None, experience: str = None, skills_description: list[str] = None, ideas: list[str] = None, current_goal: str = None, available_tools: list[dict] = None)`：更新长期上下文。
- `update_working_context(basic_info: dict[str] = {}, observation: str = None)`：更新工作上下文。

**注意**：
- 该类是一个基础类，用于派生其他具体功能的代理类。
- 可以通过调用`step`方法来进行下一步的思考/行动，并获取回复和工具调用。
- 可以通过调用`update_longterm_context`和`update_working_context`方法来更新代理的上下文。

**输出示例**：
```python
agent = BaseAgent(config, profile)
print(agent)
# Output: BaseAgent

print(agent.context())
# Output: ['LongTermContext', 'None']

message = agent.get_step_message()
print(message)
# Output: "What's next?"

chat_message = agent.get_chat_message()
print(chat_message)
# Output: "Answer the user's query based on the history."

task_conclusion = agent.get_task_conclusion("task")
print(task_conclusion)
# Output: TaskConclusion(...)

history_message = agent.get_history_message(100)
print(history_message)
# Output: [{'role': 'user', 'content': 'Your past thoughts & actions:\n...'}, {'role': 'assistant', 'content': '...'}, {'role': 'system', 'content': '...'}]

replies, tool_calls = agent.step()
print(replies)
# Output: [{'thought_type': 'simple_thought', 'thought': '...'}]
print(tool_calls)
# Output: [ToolCall(...)]

agent.update_tool_calls_output(tool_calls)
agent.update_longterm_context(user_message="...", experience="...", skills_description=["..."])
agent.update_working_context(basic_info={"Time": "..."}, observation="...")
```
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化 XAgent 类的实例。

该函数是 XAgent 类的构造函数，用于初始化一个新的 XAgent 实例。它接收多个参数，用于配置代理的行为和上下文。

- `config`: 一个 XAgentConfig 类型的对象，包含了 XAgent 的配置信息。
- `profile`: 一个字符串，代表代理的配置文件，用于个性化代理的行为。
- `step_prompt`: 一个字符串，默认值为 "What's next?"，用于询问下一步的操作提示。
- `chat_prompt`: 一个字符串，默认值为 "Answer the user's query based on the history."，用于聊天时基于历史信息回答用户问题的提示。
- `conclusion_prompt`: 一个字符串，默认值为 `CONCLUSION` 常量，用于结束对话时的提示。
- `default_replies`: 一个字面量类型，可选值为 "action_reasoning"、"simple_thought" 或 "observation_thought"，默认值为 "simple_thought"，用于设置默认的回复类型。

在函数体内部，首先将传入的参数赋值给实例变量。然后，初始化了两个上下文对象：`longterm_context` 和 `working_context`。`longterm_context` 是一个长期上下文对象，而 `working_context` 是一个工作上下文对象，用于存储当前任务的状态。

如果 `default_replies` 参数不为空，则通过 `function_manager.get_function_schema` 方法获取相应的函数模式，并赋值给 `self.default_replies`。

此外，函数还初始化了两个历史记录属性：`history_working_context` 和 `history_tool_calls`。`history_working_context` 是一个工作上下文的列表，用于存储历史工作上下文；`history_tool_calls` 是一个字典，键为字符串，值为 ToolCall 对象，用于存储历史工具调用记录。

最后，函数获取了结束工作的参数模式，并赋值给 `self.conclusion_arguments`，同时初始化了两个任务相关的私有变量 `_tool_call_summary_task` 和 `_context_summary_task`，这两个变量可能用于后续的任务汇总或上下文汇总。

**注意**：
- 在使用此构造函数时，需要确保传入的 `config` 参数是一个有效的 XAgentConfig 对象。
- `profile` 参数应该根据实际情况提供，以便个性化代理的行为。
- `default_replies` 参数应该根据需要选择合适的回复类型。
- 该构造函数可能依赖于 `function_manager` 模块来获取函数模式，因此在使用前需要确保该模块已正确配置和初始化。
## FunctionDef __str__
**__str__ 方法**: 该方法的功能是返回对象的类名。

当在Python中定义一个类时，我们经常会重写内置的`__str__`方法来为对象提供一个更加友好的字符串表示。在这段代码中，`__str__`方法被定义为返回对象所属类的名称。

具体来说，`self.__class__.__name__`这个表达式做了以下几件事情：
- `self`代表类的实例本身。
- `self.__class__`获取到该实例的类。
- `__name__`属性则是获取到该类的名字，这是一个字符串。

因此，当你打印一个对象或者在需要字符串表示的地方使用对象时，Python会自动调用这个`__str__`方法，返回类的名称。

**注意**：在使用`__str__`方法时，需要确保它返回的是一个字符串类型的值，否则在将对象转换为字符串时可能会遇到错误。

**输出示例**：如果对象属于`MyClass`类，那么调用`__str__`方法的返回值可能会是"MyClass"。
## AsyncFunctionDef lazy_init
**lazy_init函数**: 该函数的作用是初始化XAgent对象的配置和上下文。

该`lazy_init`函数是一个异步方法，它接收一个`XAgentConfig`类型的参数`config`，用于初始化XAgent对象的配置和上下文。函数的主要步骤如下：

1. 将传入的`config`参数赋值给`self.config`，以便在XAgent对象中使用这个配置信息。
2. 根据`config`中的`experiment.maintain_history_context`设置，决定是否初始化历史工作上下文(`self.history_working_context`)和历史工具调用(`self.history_tool_calls`)。如果`maintain_history_context`为`False`，则会创建这两个属性，分别用于存储历史上下文和历史工具调用的信息。
3. 检查`self._tool_call_summary_task`是否为协程，如果是，则等待该协程完成。完成后，将`self._tool_call_summary_task`设置为`None`。
4. 检查`self._context_summary_task`是否为协程，如果是，则等待该协程完成。完成后，将`self._context_summary_task`设置为`None`。

在项目中，`lazy_init`函数被`XAgent/engines/base.py`文件中的同名异步方法调用。在这个调用中，首先检查是否传入了`config`参数，如果传入了，则使用该参数更新`self.config`。接着，如果`self.agent`不为`None`，则调用`self.agent`的`lazy_init`方法，并传入配置。最后，根据`get_available_tools`参数的值决定是否调用`self.get_available_tools()`方法来获取可用工具。

**注意**:
- 由于`lazy_init`是一个异步方法，因此在调用时需要使用`await`关键字。
- 在使用`lazy_init`方法之前，需要确保传入的`config`参数是有效的`XAgentConfig`对象。
- 该方法中的`self._tool_call_summary_task`和`self._context_summary_task`属性应该是在其他地方定义的协程任务，这里需要确保它们在初始化时被正确处理。
- 在调用`lazy_init`方法时，需要注意`get_available_tools`参数的默认值和实际需求，以决定是否需要获取可用工具列表。
## FunctionDef context
**context函数**: 此函数的功能是返回代理的完整上下文。

此函数`context`定义在一个代理类中，它的主要作用是获取当前代理的完整上下文信息。上下文信息通常用于理解和追踪代理的状态，以及它在执行任务时所依赖的环境和历史信息。

在代码实现中，`context`函数返回一个包含两个字符串元素的列表。这两个字符串分别代表了代理的长期上下文(`longterm_context`)和工作上下文(`working_context`)。这里使用了`str`函数将上下文对象转换为字符串，这可能是因为上下文对象本身包含了复杂的数据结构，而字符串形式更便于查看和传输。

**注意**：
- 在使用此函数时，需要确保代理类中已经正确初始化了`longterm_context`和`working_context`这两个属性，否则在调用此函数时可能会遇到属性不存在的错误。
- 返回的列表中，第一个元素对应长期上下文，第二个元素对应工作上下文，使用者应当根据实际需要来处理这两个上下文信息。

**输出示例**：
假设代理的长期上下文是一个包含历史任务记录的复杂对象，工作上下文是一个包含当前任务状态的对象，那么函数的返回值可能看起来像这样：
```python
['{"task_id": "123", "previous_results": ["result1", "result2"]}', '{"current_task": "task456", "status": "in_progress"}']
```
在这个示例中，长期上下文被转换成了一个包含任务ID和之前任务结果的JSON字符串，而工作上下文被转换成了一个包含当前任务信息和状态的JSON字符串。
## AsyncFunctionDef get_step_message
**get_step_message函数**: 此函数的功能是返回下一步操作的消息。

该`get_step_message`函数是一个异步方法，定义在`XAgent/agent/base.py`文件中。它的主要作用是根据给定的模式（`mode`），返回相应的消息字符串。函数接受一个名为`mode`的参数，该参数有两个可能的值："default"和"conclusion"。此外，函数还接受任意数量的关键字参数（`**kwargs`），这些参数将用于格式化返回的消息字符串。

当`mode`参数设置为"conclusion"时，函数会返回一个`conclusion_prompt`的深拷贝，并使用`format`方法将`**kwargs`中的参数填充到该字符串中。如果`mode`参数设置为"default"或者不提供（默认值为"default"），则会返回一个`step_prompt`的深拷贝，并同样使用`format`方法进行格式化。

在`XAgent/agent/base.py`文件的`step`方法中，`get_step_message`函数被用来构造用户角色的消息内容。这个消息内容将被用作与系统交互的一部分，例如在聊天模式下或者执行步骤时，提供给用户的提示或者信息。

在`XAgent/agent/plan.py`文件中，`get_step_message`函数被重写以添加一个新的模式"generate"。当`mode`参数设置为"generate"时，函数会返回一个`inital_prompt`的深拷贝，并使用`format`方法进行格式化。如果`mode`不是"generate"，则会调用基类的`get_step_message`方法。

**注意**:
- 在调用`get_step_message`函数时，需要确保`conclusion_prompt`和`step_prompt`属性已经被正确地初始化，并且包含了可以被格式化的字符串。
- 由于函数使用了`deepcopy`，如果`conclusion_prompt`或`step_prompt`包含了复杂的数据结构，可能会对性能有一定影响。
- 函数是异步的，因此在调用时需要使用`await`关键字。

**输出示例**:
假设`step_prompt`的值为"下一步操作是：{action}"，并且调用函数时传入了`kwargs`参数`{'action': '开始任务'}`，那么函数的返回值可能如下：
```
下一步操作是：开始任务
```
如果`mode`参数设置为"conclusion"，并且`conclusion_prompt`的值为"任务完成，结果是：{result}"，同时传入了`kwargs`参数`{'result': '成功'}`，那么函数的返回值可能如下：
```
任务完成，结果是：成功
```
## AsyncFunctionDef get_chat_message
**get_chat_message函数**: 此函数的功能是返回用于聊天的消息。

此函数是一个异步方法，定义在`XAgent/agent/base.py`文件中的`Base`类里。它的主要作用是根据传入的关键字参数（`kwargs`），返回一个格式化后的聊天提示信息。这个函数使用了`deepcopy`来确保返回的字符串是原始聊天提示信息的一个深拷贝，这样做可以避免在原始提示信息上进行修改，保持其不变性。

在`get_chat_message`函数中，`self.chat_prompt`是一个预定义的模板字符串，它可能包含了一些占位符，这些占位符将会被`format`方法替换成`kwargs`中对应的值。例如，如果`self.chat_prompt`是`"你好，{name}！"`，那么调用`get_chat_message(name="小明")`将会返回`"你好，小明！"`。

在项目中，`get_chat_message`函数被调用于`Base`类的`step`方法中。在`step`方法中，如果`chat_mode`参数被设置为`True`，则会调用`get_chat_message`函数来构造用户角色的消息内容。这表明`get_chat_message`函数在处理聊天模式下的用户输入时起到了关键作用。

**注意**：
- 由于`get_chat_message`是一个异步方法，因此在调用时需要使用`await`关键字。
- 调用此函数时需要确保`self.chat_prompt`已经被正确初始化，且包含了适当的格式化占位符。
- 传入的关键字参数（`kwargs`）必须与`self.chat_prompt`中的占位符相匹配，否则`format`方法将会抛出异常。

**输出示例**：
假设`self.chat_prompt`为`"你好，{name}！今天的天气是{weather}。"`，调用`await get_chat_message(name="小明", weather="晴朗")`可能会返回：
```
你好，小明！今天的天气是晴朗。
```
## AsyncFunctionDef get_task_conclusion
**get_task_conclusion函数**：此函数的功能是返回任务的结论。

该函数是一个异步函数，接受一个任务字符串和一个可选的任务概览参数，并返回一个TaskConclusion对象作为结果。函数的具体实现如下：

```python
async def get_task_conclusion(self, task: str, tasks_overview="`wrapped`") -> TaskConclusion:
    """Return the conclusion of the task."""
    replies, tool_calls = await self.step(
        step_arguments={"mode": "conclusion",
                        "task": task,
                        "tasks_overview": tasks_overview},
        replies=self.conclusion_arguments,
        tools=[],
        setup_summary_tasks=False
    )
    replies = replies[0]  # skip other
    task_conclusion = TaskConclusion(**replies)
    self.longterm_context.past_tasks.append(task_conclusion)
    return task_conclusion
```

该函数首先调用了`self.step`方法，传入了一些参数，包括`mode`、`task`和`tasks_overview`等。然后，根据返回的结果，创建了一个TaskConclusion对象，并将其添加到`self.longterm_context.past_tasks`列表中。最后，将TaskConclusion对象作为函数的返回值。

**注意**：在使用该函数时，需要注意以下几点：
- 该函数是一个异步函数，需要使用`await`关键字进行调用。
- 函数的第一个参数`task`是一个任务字符串，表示要获取结论的任务。
- 函数的第二个参数`tasks_overview`是一个可选参数，表示任务的概览信息，默认值为"`wrapped`"。
- 函数的返回值是一个TaskConclusion对象，表示任务的结论。

**输出示例**：下面是该函数的一个可能的返回值的示例：
```python
{
    "status": "success",
    "result": "Task completed successfully",
    "details": {
        "duration": "00:05:23",
        "output": "Some output"
    }
}
```
## FunctionDef _setup_summary_tasks
**_setup_summary_tasks函数**: 此函数的功能是设置摘要任务。

该函数用于设置摘要任务，根据配置文件中的设置来生成工具调用输出摘要和工作上下文摘要。首先，函数会检查配置文件中的摘要功能是否启用。如果未启用，则直接返回None。如果启用了摘要功能，则根据配置文件中的设置创建相应的摘要任务。

如果配置文件中设置了工具调用摘要(summarycfg.tool_calls)且尚未创建工具调用摘要任务(self._tool_call_summary_task is None)，则会创建一个工具调用摘要任务。该任务将调用summarize_tool_calls函数，传入长期上下文(longterm_context)、历史工作上下文中非空的自我检查(self_examination)列表、历史工具调用列表(self.history_tool_calls.values())作为参数。该任务将异步执行，生成工具调用输出摘要。

如果配置文件中设置了工作上下文摘要(summarycfg.working_context)且尚未创建工作上下文摘要任务(self._context_summary_task is None)，则会创建一个工作上下文摘要任务。该任务将调用summarize_working_context函数，传入长期上下文(longterm_context)和最新的工作上下文(self.history_working_context[-1])作为参数。该任务将异步执行，生成工作上下文摘要。

**注意**: 使用此代码需要注意以下几点:
- 需要在配置文件中启用摘要功能。
- 需要正确设置工具调用摘要(summarycfg.tool_calls)和工作上下文摘要(summarycfg.working_context)的值。

**输出示例**:
```
[
    {
        "role": "user",
        "content": "Your past thoughts & actions:\n\n# Basic Info\n{working_context.basic_info}\n# Observation\n{working_context.observation}\n# Tool Calls\n{tool_calls_description[tool_call.id]}\nAlso you get {len(tool_call_images)} image:\n{tool_call_images}"
    },
    {
        "role": "assistant",
        "content": "Your action results:\n# Tool Calls\n{str(tool_call)}"
    },
    {
        "role": "system",
        "content": "Your past thoughts & actions:\n\n# Basic Info\n{working_context.basic_info}\n# Observation\n{working_context.observation}\n# Tool Calls\n{tool_calls_description[tool_call.id]}\nAlso you get {len(tool_call_images)} image:\n{tool_call_images}"
    }
]
```
## AsyncFunctionDef get_history_message
**get_history_message函数**：此函数的功能是返回具有过去工作内存的历史消息。

该函数接受一个max_tokens参数，用于限制返回消息的最大令牌数。函数返回一个包含历史消息的列表，每个消息都是一个字典，包含"role"和"content"两个键。

函数的具体实现如下：

1. 首先，定义一个空列表history_overview，用于存储历史消息的概览信息。
2. 初始化current_tokens为0，用于记录当前已使用的令牌数。
3. 获取配置文件中的summarycfg，用于判断是否启用摘要功能。
4. 如果启用了摘要功能(summarycfg.enable为True)，则执行以下步骤：
   - 调用_setup_summary_tasks()函数设置摘要任务。
   - 如果summarycfg.tool_calls为True，则获取工具调用的摘要信息，包括工具调用的描述、推理和令牌数，并更新latest tool call的描述。
   - 如果summarycfg.working_context为True，则获取工作上下文的摘要信息，并更新工作上下文的_summary和_self_examination属性。
   - 构建历史消息：
     - 遍历历史工作上下文列表，从最近的工作上下文开始。
     - 如果当前已使用的令牌数超过了max_tokens，则跳出循环。
     - 构建消息的内容，包括过去的思考和行动。
     - 如果工作上下文中存在工具调用，则添加工具调用的描述。
     - 如果配置中启用了多模态图像功能(self.config.multimodal.image.enable为True)，则添加工具调用的图像。
     - 将消息的内容进行截断，使其不超过max_tokens-current_tokens个令牌。
     - 将消息添加到history_overview列表中。
5. 如果未启用摘要功能(summarycfg.enable为False)，则执行以下步骤：
   - 遍历历史工作上下文列表，从最近的工作上下文开始。
   - 如果当前已使用的令牌数超过了max_tokens，则跳出循环。
   - 构建消息的内容，包括工具调用的结果和助手上下文的信息。
   - 如果配置中启用了多模态图像功能(self.config.multimodal.image.enable为True)，则添加工具调用的图像。
   - 将消息的内容进行截断，使其不超过max_tokens-current_tokens个令牌。
   - 将消息添加到history_overview列表中。
6. 将history_overview列表反转，并返回其中内容不为空的消息。

**注意**：在使用该函数时需要注意以下几点：
- max_tokens参数用于限制返回消息的最大令牌数，可以根据实际需求进行调整。
- 函数的返回值是一个包含历史消息的列表，每个消息都是一个字典，包含"role"和"content"两个键。

**输出示例**：以下是该函数可能返回的结果示例：
```python
[
    {
        "role": "user",
        "content": "Your past thoughts & actions:\nThought 1\nThought 2\n# Tool Calls\nTool Call 1\nTool Call 2"
    },
    {
        "role": "assistant",
        "content": "Assistant context:\nContext 1\nContext 2"
    },
    {
        "role": "system",
        "content": "Basic Info\nInfo 1\nInfo 2\nObservation\nObservation 1\nObservation 2"
    }
]
```
## AsyncFunctionDef step
**step函数**：这个函数的功能是在更新了agent上下文之后，agent将迈向下一个思考/动作。返回在这个步骤中生成的回复和工具调用。在这个步骤中，提供的回复、工具和工具选择将被临时使用，并在此步骤后重置为默认值。您还可以为这个函数添加更多的关键字参数，它们将被传递给完成函数。

该函数的详细代码分析和描述如下：

- 首先，根据提供的参数构造消息。
- 然后，计算消息中的令牌数。
- 接下来，根据历史消息的长度获取历史消息。
- 将长期消息、历史消息和工作消息合并为一个消息列表。
- 如果提供了消息参数，则将其添加到消息列表中。
- 复制回复、工具选择和工具列表。
- 调用objgenerator.chatcompletion函数，传递消息、回复、工具和工具选择等参数，获取回复和工具调用的结果。
- 如果需要更新工作上下文，则将工具调用添加到工作上下文中。
- 如果需要更新工作上下文，则将回复和工具调用添加到工作上下文中，并将工作上下文添加到历史工作上下文列表中。
- 如果需要设置摘要任务，则调用_setup_summary_tasks函数。
- 遍历回复列表，打印出回复的内容。
- 如果回复列表为空，则打印出警告信息。
- 将步骤信息写入记录器。
- 返回回复列表和工具调用列表。

**注意**：在调用step函数时，可以提供一些关键字参数来定制函数的行为。此外，step函数还会更新工作上下文、记录回复和工具调用，并将步骤信息写入记录器。

**输出示例**：
```
回复列表：[
    {
        "role": "assistant",
        "content": "这是一个回复"
    },
    {
        "role": "assistant",
        "content": "这是另一个回复"
    }
]
工具调用列表：[
    {
        "id": 1,
        "name": "tool1",
        "arguments": {
            "arg1": "value1",
            "arg2": "value2"
        }
    },
    {
        "id": 2,
        "name": "tool2",
        "arguments": {
            "arg1": "value1",
            "arg2": "value2"
        }
    }
]
```
## AsyncFunctionDef _get_basic_info
**_get_basic_info函数**: 该函数的功能是获取基本信息。

_get_basic_info函数是一个异步函数，它的作用是生成一个包含当前时间和步骤ID的字典。这个字典包含两个键值对，"Time"键对应的值是当前的时间，格式为"年-月-日 星期 时:分:秒"；"Step Id"键对应的值是一个字符串，表示当前历史工作上下文的长度，即步骤编号。

函数内部首先导入了time模块，用于获取和格式化当前时间。然后，创建了一个名为basic_info的字典，其中包含两个键值对。"Time"键的值通过time模块的strftime函数和localtime函数获取当前的本地时间，并按照指定的格式进行格式化。"Step Id"键的值是一个格式化字符串，其值为"No."加上self.history_working_context列表的长度，表示当前是第几步操作。

在项目中，_get_basic_info函数被update_working_context函数调用。update_working_context函数是用来更新代理工作上下文的。它首先创建一个新的工作上下文对象，然后调用_get_basic_info函数获取基本信息，并将其与传入的basic_info字典合并。之后，它将合并后的信息格式化为字符串，并设置到工作上下文的basic_info属性中。如果传入了observation参数，还会将其设置到工作上下文的observation属性中。

**注意**：
- 由于_get_basic_info是一个异步函数，因此在调用时需要使用await关键字。
- 函数返回的字典中的值是字符串类型，调用者需要注意处理类型匹配问题。

**输出示例**：
调用_get_basic_info函数可能返回的字典示例如下：
```python
{
    "Time": "2023-04-01 周六 15:45:30",
    "Step Id": "No.5"
}
```
在这个示例中，"Time"键对应的值是一个格式化的时间字符串，"Step Id"键对应的值表示这是第五步操作。
## AsyncFunctionDef update_tool_calls_output
**update_tool_calls_output函数**：此函数的功能是更新代理工具调用的输出，并返回工具输出的观察结果。

该函数接受一个工具调用列表作为参数，列表中的每个元素都是一个ToolCall对象，表示一个工具调用。函数首先创建一个空的new_observation列表，用于存储更新后的工具调用的观察结果。

然后，函数通过遍历传入的工具调用列表，找到历史工具调用中与当前工具调用具有相同id的工具调用对象。然后，将当前工具调用的输出和状态更新到历史工具调用对象中，并将其转换为字符串后添加到new_observation列表中。

最后，函数将new_observation列表中的元素使用换行符连接起来，并作为字符串返回。

**注意**：在使用此函数时需要注意以下几点：
- 传入的tool_calls参数必须是一个ToolCall对象的列表。
- 传入的tool_calls列表中的每个ToolCall对象必须具有id、output和status属性。
- 函数会根据传入的tool_calls列表中的id来查找历史工具调用对象，并更新其output和status属性。

**输出示例**：假设传入的tool_calls列表中有两个工具调用对象，其id分别为1和2，输出示例可能如下所示：
```
ToolCall(id=1, output='output1', status='status1')
ToolCall(id=2, output='output2', status='status2')
```
## AsyncFunctionDef update_longterm_context
**update_longterm_context函数**：此函数用于更新代理的长期上下文。

该函数接受以下参数：
- user_message（可选）：用户消息，字符串类型。
- experience（可选）：经验，字符串类型。
- skills_description（可选）：技能描述，字符串列表类型。
- ideas（可选）：想法，字符串列表类型。
- current_goal（可选）：当前目标，字符串类型。
- available_tools（可选）：可用工具，字典列表类型。

该函数的功能是根据传入的参数更新代理的长期上下文。具体功能如下：
- 如果传入了user_message参数，则将其添加到代理的长期上下文的user_message列表中。
- 如果传入了experience参数，则将其设置为代理的长期上下文的experience属性。
- 如果传入了skills_description参数，则将其设置为代理的长期上下文的skills_description属性。
- 如果传入了ideas参数，则将其设置为代理的长期上下文的ideas属性。
- 如果传入了current_goal参数，则将其设置为代理的长期上下文的current_goal属性。
- 如果传入了available_tools参数，则将其设置为代理的长期上下文的available_tools属性。

**注意**：在使用此代码时需要注意以下几点：
- user_message参数是可选的，如果不传入该参数，则不会对代理的长期上下文进行更新。
- 参数的类型需要符合函数定义中的类型要求，否则可能会导致错误。
- 该函数是异步函数，需要使用`await`关键字进行调用。
## AsyncFunctionDef update_working_context
**update_working_context函数**：此函数的功能是更新代理工作上下文。如果参数为None，则使用默认更新。

此函数接受两个参数：
- basic_info：一个字典类型的参数，用于更新代理的基本信息。默认为空字典。
- observation：一个字符串类型的参数，用于更新代理的观察信息。默认为None。

函数内部逻辑如下：
1. 创建一个WorkingContext对象，并将其赋值给self.working_context。
2. 调用_get_basic_info()函数获取基本信息，并将其保存在infos变量中。
3. 将basic_info参数中的键值对更新到infos中。
4. 将infos中的非空值拼接成字符串，并赋值给self.working_context.basic_info。
5. 如果observation参数不为None，则将其赋值给self.working_context.observation。
6. 返回True表示更新成功。

**注意**：
- 在调用此函数之前，需要确保已经初始化了self.working_context对象。
- 在调用此函数之前，需要确保已经定义了_get_basic_info()函数。

**输出示例**：
```
True
```
***
