# ClassDef LLMStatusCode
**LLMStatusCode功能**: 该类的功能是定义了一个枚举类型，用于表示低级语言模型（LLM）操作的状态码。

LLMStatusCode类继承自Enum，这是Python标准库中的一个用于创建枚举的类。枚举（Enumerations）是一种组织具有命名值的数据类型的方式，常用于定义一组有限数量的可能值，并为每个值赋予一个易于理解的名字。

在LLMStatusCode类中定义了两个状态码：
- `SUCCESS = 0`：表示操作成功完成。在使用低级语言模型进行操作时，如果没有发生错误，通常会返回这个状态码。
- `ERROR = 1`：表示操作中出现了错误。如果在执行过程中遇到任何问题，例如输入无效或模型无法处理的请求，就会返回这个状态码。

使用枚举类型的好处是它提供了一种类型安全的方式来处理固定集合的值。开发者可以通过枚举成员的名字来引用这些值，而不是使用原始的数值，这样可以提高代码的可读性和可维护性。

**注意**：
- 在使用LLMStatusCode时，应该通过枚举成员的名字来引用状态码，而不是直接使用数值，这样可以避免魔法数字（magic numbers）的出现，并使代码更加清晰。
- 由于LLMStatusCode是一个枚举类，它的成员是不可变的，不能被修改。
- 当需要对LLM操作的结果进行判断时，可以使用这个枚举类来进行比较，例如：`if result == LLMStatusCode.SUCCESS:`。
- 在将枚举值序列化为JSON或其他格式时，可能需要特别处理，因为直接序列化枚举成员可能会得到成员的名称而不是其值。
***
# ClassDef ToolType
**ToolType类**：这个类的功能是定义工具的类型。

ToolType类是一个枚举类，用于表示工具的类型。它定义了以下几种类型：
- Default：默认类型
- BuiltIn：内置类型
- ToolServer：工具服务器类型
- Rapid：Rapid类型
- ToolCombo：工具组合类型
- N8N：N8N类型
- Custom：自定义类型

这个类还重写了__hash__方法，用于返回对象的哈希值。

注意：在使用ToolType类时，可以通过ToolType.Default、ToolType.BuiltIn等方式来引用具体的类型。

输出示例：
```
ToolType.Default
ToolType.BuiltIn
ToolType.ToolServer
ToolType.Rapid
ToolType.ToolCombo
ToolType.N8N
ToolType.Custom
```
## FunctionDef __hash__
**__hash__函数**: 该函数的功能是计算对象的哈希值。

该`__hash__`方法是一个特殊方法，用于获取对象的哈希值。在Python中，当对象需要被哈希时（例如，当你将对象添加到一个集合或用作字典的键时），这个方法会被调用。`__hash__`方法返回一个整数，代表对象的哈希值。

在这段代码中，`__hash__`方法通过调用内置的`hash`函数，并传递对象的`value`属性作为参数来计算哈希值。这意味着对象的哈希值是基于其`value`属性的哈希值。

这种实现方式通常用于不可变类型的对象，确保对象在其生命周期内哈希值保持不变，这是字典键和集合成员所必需的。

**注意**:
- 确保对象的`value`属性是可哈希的，否则调用`hash`函数时会抛出异常。
- 如果对象的`value`属性在对象的生命周期内发生变化，那么对象的哈希值也会变化，这可能会破坏字典或集合的内部结构。因此，通常只有不可变对象才应该实现`__hash__`方法。
- 如果一个类定义了`__eq__`方法，它也应该定义`__hash__`方法，以便对象可以用作哈希表的键。

**输出示例**:
假设对象的`value`属性是字符串"example"，那么`__hash__`方法可能返回的哈希值示例为：
```python
>>> obj = CustomClass("example")
>>> hash(obj)
-9223372036854775807  # 假设的哈希值，实际值会根据系统和Python版本等因素而有所不同
```
***
# ClassDef ToolCallStatusCode
**ToolCallStatusCode类**

该类用于表示工具调用的状态码。

函数：
- `__str__(self)`: 返回状态码的字符串表示。
- `color(self)`: 返回状态码对应的颜色。

**注意**：
- `__str__`函数返回状态码的类名和名称。
- `color`函数根据状态码的名称返回对应的颜色。

**输出示例**：
```
ToolCallStatusCode.INTERRUPTED: INTERRUPTED
```

**ToolCallStatusCode类的功能**：该类用于表示工具调用的状态码。状态码用于标识工具调用的执行状态，包括中断、待处理、失败、成功等不同状态。

该类包含以下状态码：
- `INTERRUPTED`: 工具调用被中断
- `PENDING`: 工具调用待处理
- `FAILED`: 工具调用失败
- `SUCCESS`: 工具调用成功
- `FORMAT_ERROR`: 工具调用格式错误
- `HALLUCINATE_NAME`: 工具调用名称错误
- `OTHER_ERROR`: 其他错误
- `TIMEOUT_ERROR`: 工具调用超时错误
- `TIME_LIMIT_EXCEEDED`: 工具调用超出时间限制
- `SERVER_ERROR`: 服务器错误

**注意**：
- `__str__`函数返回状态码的类名和名称。
- `color`函数根据状态码的名称返回对应的颜色。

**输出示例**：
```
ToolCallStatusCode.INTERRUPTED: INTERRUPTED
```
## FunctionDef __str__
**__str__ 方法**: 该方法的功能是将对象转换为其类名和名称的字符串表示形式。

`__str__` 方法是 Python 中的一个特殊方法，用于定义一个对象的“非正式”或可打印的字符串表示。当你尝试使用 `print()` 函数打印一个对象，或者在字符串格式化操作中使用对象时，Python 会调用这个方法来获取对象的字符串表示。

在这段代码中，`__str__` 方法被定义为返回一个字符串，该字符串由对象的类名和对象的 `name` 属性值组成，中间以冒号和空格分隔。这样做可以提供关于对象的直观信息，便于调试和日志记录。

具体来说，`self.__class__.__name__` 获取当前对象的类名，`self.name` 假设是当前对象的一个属性，代表对象的名称。这两部分通过字符串连接操作符 `+` 连接起来，并在类名后面加上了一个冒号和空格，以提供清晰的格式。

**注意**：使用这个方法时，需要确保对象有一个名为 `name` 的属性，否则会抛出 `AttributeError`。此外，这个方法应该返回一个字符串，如果 `name` 属性不是字符串类型，可能需要先将其转换为字符串。

**输出示例**：如果有一个类 `ExampleClass`，其实例的 `name` 属性值为 `"example_name"`，那么调用这个实例的 `__str__` 方法将返回 `"ExampleClass: example_name"`。
***
# ClassDef PlanOperationStatusCode
**PlanOperationStatusCode类的功能**: 该类用于定义计划操作的状态码。

该类包含以下状态码：
- MODIFY_SUCCESS：修改成功
- MODIFY_FORMER_PLAN：修改之前的计划
- PLAN_OPERATION_NOT_FOUND：找不到计划操作
- TARGET_SUBTASK_NOT_FOUND：找不到目标子任务
- PLAN_REFINE_EXIT：计划细化退出
- OTHER_ERROR：其他错误

该类还包含一个color()方法，用于根据状态码返回对应的颜色。具体实现如下：

- 如果状态码为MODIFY_SUCCESS，则返回绿色
- 如果状态码为MODIFY_FORMER_PLAN、PLAN_OPERATION_NOT_FOUND或TARGET_SUBTASK_NOT_FOUND，则返回红色
- 如果状态码为PLAN_REFINE_EXIT，则返回蓝色
- 如果状态码为其他情况，则返回红色

**注意**: 在使用该类时需要注意以下几点：
- 可以通过调用color()方法获取状态码对应的颜色

**输出示例**:
```
PlanOperationStatusCode.MODIFY_SUCCESS.color()的输出结果为：绿色
PlanOperationStatusCode.MODIFY_FORMER_PLAN.color()的输出结果为：红色
PlanOperationStatusCode.PLAN_OPERATION_NOT_FOUND.color()的输出结果为：红色
PlanOperationStatusCode.TARGET_SUBTASK_NOT_FOUND.color()的输出结果为：红色
PlanOperationStatusCode.PLAN_REFINE_EXIT.color()的输出结果为：蓝色
PlanOperationStatusCode.OTHER_ERROR.color()的输出结果为：红色
```
***
# ClassDef EngineExecutionStatusCode
**EngineExecutionStatusCode类的功能**: 这个类定义了引擎执行状态码的枚举类型，用于表示引擎执行过程中的不同状态。

引擎执行状态码包括以下几种状态：
- DOING: 表示引擎正在执行中。
- SUCCESS: 表示引擎执行成功。
- FAIL: 表示引擎执行失败。
- HAVE_AT_LEAST_ONE_ANSWER: 表示引擎至少有一个答案。

这个类的作用是为引擎执行过程中的不同状态提供一个标识，方便在代码中进行状态判断和处理。

**注意**: 在使用这个类时，可以通过引用类名和枚举值来表示不同的状态，例如`EngineExecutionStatusCode.SUCCESS`表示引擎执行成功的状态。可以根据具体的业务逻辑和需求，使用这些状态码来进行相应的处理和判断。

请注意：
- 生成的文档内容不应包含Markdown的标题和分割线语法。
- 文档主要使用中文撰写，如果需要，可以在分析和描述中使用一些英文单词以增强文档的可读性，因为不需要将函数名或变量名翻译成目标语言。
***
# ClassDef TaskStatusCode
**TaskStatusCode类函数**: 这个类的功能是定义任务的状态码。

TaskStatusCode类是一个枚举类，用于表示任务的不同状态。它包含以下几个状态码：

- TODO: 代表任务尚未开始。
- DOING: 代表任务正在进行中。
- SUCCESS: 代表任务成功完成。
- FAIL: 代表任务失败。
- SPLIT: 代表任务被拆分。

此外，TaskStatusCode类还定义了一个color()方法，用于根据任务状态返回对应的颜色。具体的颜色映射如下：

- SUCCESS: 绿色
- FAIL: 红色
- DOING: 蓝色
- SPLIT: 黄色
- 其他状态: 白色

**注意**: 在使用TaskStatusCode类时，可以通过调用color()方法获取任务状态对应的颜色。

**输出示例**:
```
from colorama import Fore

status = TaskStatusCode.SUCCESS
print(status.color())  # 输出: 绿色
```
## FunctionDef color
**color函数**: 此函数的功能是根据枚举成员的名称返回对应的颜色代码。

color函数是一个根据枚举成员的名称返回特定颜色代码的方法。这个方法通常用于在命令行界面中以不同颜色显示不同状态的文本，以提高可读性和用户体验。该函数使用了Python的模式匹配语法（match-case），根据枚举成员的名称匹配并返回相应的颜色代码。

在这个函数中，枚举成员的名称与颜色代码的对应关系如下：
- "SUCCESS" 对应 `Fore.GREEN`，表示绿色，通常用于表示操作成功。
- "FAIL" 对应 `Fore.RED`，表示红色，通常用于表示操作失败。
- "DOING" 对应 `Fore.BLUE`，表示蓝色，通常用于表示操作正在进行中。
- "SPLIT" 对应 `Fore.YELLOW`，表示黄色，可能用于表示操作处于分割或等待状态。
- 其他情况对应 `Fore.WHITE`，表示白色，作为默认颜色。

在项目中调用此函数的情况，例如在`XAgent/engines/plan.py`文件中，`color`函数被用来设置日志文本的颜色。在执行任务时，会根据任务的状态（例如"DOING"、"SUCCESS"或"FAIL"）来选择相应的颜色代码，并将其作为参数传递给`recorder.log`函数，以便在命令行界面中以不同颜色高亮显示任务的执行状态。

**注意**：
- 使用此函数时，需要确保`self.name`是一个有效的枚举成员名称，否则将返回默认的白色。
- 此函数依赖于`Fore`类，该类通常来源于`colorama`库，因此在使用前需要确保已正确安装并导入了`colorama`库。

**输出示例**：
假设枚举成员的名称为"SUCCESS"，则`color`函数将返回`Fore.GREEN`。如果在命令行界面中使用此颜色代码，相应的文本将以绿色显示。
***
# ClassDef RequiredAbilities
**RequiredAbilities类的功能**: RequiredAbilities类是一个枚举类，用于定义代理所需的能力。

RequiredAbilities类定义了以下能力：
- plan：生成和修正计划
- action：执行任务
- tool_tree_search：使用工具树搜索
- plan_generation：生成计划
- plan_refer_db：参考数据库生成计划
- plan_refinement：修正计划
- task_evaluator：评估任务
- summarization：摘要生成
- reflection：反思
- pipeline_route：管道路由
- pipeline_give_params：管道参数设置

**注意**: 在使用RequiredAbilities类时，可以根据需要选择所需的能力。

在项目中，RequiredAbilities类被以下文件调用：
文件路径：XAgent/agent/plan.py
对应代码如下：
```python
class PlanAgent(BaseAgent):
    """
    PlanAgent是一个可以生成和修正计划的代理。
    """
    abilities = set([RequiredAbilities.plan_generation,
                    RequiredAbilities.plan_refinement])

    def __init__(self,
                 config: XAgentConfig,):
        super(PlanAgent, self).__init__(
            config=config,
            profile=PROFILE.format(
                max_plan_tree_width=config.execution.max_plan_tree_width,
                max_plan_tree_depth=config.execution.max_plan_tree_depth,
            ),
            step_prompt=STEP,
            chat_prompt=CHAT,
        )
        self.inital_prompt = INIT

    async def get_step_message(self, mode: Literal["default", "conclusion", "generate"] = "default", **kwargs) -> str:
        """返回下一步的消息。"""
        if mode == "generate":
            return deepcopy(self.inital_prompt).format(**kwargs)
        else:
            return await super().get_step_message(mode=mode, **kwargs)

    async def generate_plan(self, query: str, tool_choice: dict) -> list[Task]:
        """为查询生成计划。"""
        replies, tool_calls = await self.step(
            step_arguments={"mode": "generate", "query": query},
            tool_choice=tool_choice,
        )
        tool_call = tool_calls[0]
        return [Task(**task_dict) for task_dict in tool_call.arguments["subtasks"]]
```
[代码结束]
对应代码如下：
```python
class PlanAgent(BaseAgent):
    """
    PlanAgent是一个可以生成和修正计划的代理。
    """
    abilities = set([RequiredAbilities.plan_generation,
                    RequiredAbilities.plan_refinement])

    def __init__(self,
                 config: XAgentConfig,):
        super(PlanAgent, self).__init__(
            config=config,
            profile=PROFILE.format(
                max_plan_tree_width=config.execution.max_plan_tree_width,
                max_plan_tree_depth=config.execution.max_plan_tree_depth,
            ),
            step_prompt=STEP,
            chat_prompt=CHAT,
        )
        self.inital_prompt = INIT

    async def get_step_message(self, mode: Literal["default", "conclusion", "generate"] = "default", **kwargs) -> str:
        """返回下一步的消息。"""
        if mode == "generate":
            return deepcopy(self.inital_prompt).format(**kwargs)
        else:
            return await super().get_step_message(mode=mode, **kwargs)

    async def generate_plan(self, query: str, tool_choice: dict) -> list[Task]:
        """为查询生成计划。"""
        replies, tool_calls = await self.step(
            step_arguments={"mode": "generate", "query": query},
            tool_choice=tool_choice,
        )
        tool_call = tool_calls[0]
        return [Task(**task_dict) for task_dict in tool_call.arguments["subtasks"]]
```
[代码结束]
[文件结束]

文件路径：XAgent/agent/route.py
对应代码如下：
```python
class RouteAgent(BaseAgent):
    """
    RouteAgent是一个可以使用各种工具处理任务的代理。
    """
    abilities = set([RequiredAbilities.tool_tree_search,RequiredAbilities.reflection])
    def __init__(self,
                 config,):
        super(RouteAgent, self).__init__(
            config=config,
            profile=PROFILE,
            step_prompt=STEP,
            default_replies="action_reasoning"
        )
```
[代码结束]
对应代码如下：
```python
class RouteAgent(BaseAgent):
    """
    RouteAgent是一个可以使用各种工具处理任务的代理。
    """
    abilities = set([RequiredAbilities.tool_tree_search,RequiredAbilities.reflection])
    def __init__(self,
                 config,):
        super(RouteAgent, self).__init__(
            config=config,
            profile=PROFILE,
            step_prompt=STEP,
            default_replies="action_reasoning"
        )
```
[代码结束]
[文件结束]

文件路径：XAgent/agent/thread/tool_agent.py
对应代码如下：
```python
class ToolThreadAgent(BaseThreadAgent):
    abilities = set([RequiredAbilities.tool_tree_search])
    
    def __init__(self, config, thread_id: str = None):
        
        super().__init__(
            config, 
            SYSTEM_PROMPT, 
            TASK_PROMPT_TEMPLATE, 
            thread_id)
```
[代码结束]
[文件结束]

文件路径：XAgent/agent/tool.py
对应代码如下：
```python
class ToolAgent(BaseAgent):
    """
    ToolAgent是一个可以使用各种工具处理任务的代理。
    """
    abilities = set([RequiredAbilities.tool_tree_search,
                    RequiredAbilities.reflection])

    def __init__(self,
                 config: XAgentConfig,):
        super(ToolAgent, self).__init__(
            config=config,
            profile=PROFILE,
            step_prompt=STEP.format(
                max_length=config.execution.max_chain_length,
                human_help_prompt="如果需要，可以寻求人类帮助。" if config.execution.enable_ask_human_for_help else "当前无法获得人类帮助。"
            ),
            chat_prompt=CHAT,
            default_replies="observation_thought"
        )
```
[代码结束]
对应代码如下：
```python
class ToolAgent(BaseAgent):
    """
    ToolAgent是一个可以使用各种工具处理任务的代理。
    """
    abilities = set([RequiredAbilities.tool_tree_search,
                    RequiredAbilities.reflection])

    def __init__(self,
                 config: XAgentConfig,):
        super(ToolAgent, self).__init__(
            config=config,
            profile=PROFILE,
            step_prompt=STEP.format(
                max_length=config.execution.max_chain_length,
                human_help_prompt="如果需要，可以寻求人类帮助。" if config.execution.enable_ask_human_for_help else "当前无法获得人类帮助。"
            ),
            chat_prompt=CHAT,
            default_replies="observation_thought"
        )
```
[代码结束]
[文件结束]

**注意**: RequiredAbilities类是一个枚举类，用于定义代理所需的能力。在使用RequiredAbilities类时，可以根据需要选择所需的能力。在项目中，RequiredAbilities类被多个文件调用，用于定义代理的能力。
***
# ClassDef InterationJudge
**InterationJudge类的功能**: 该类用于判断用户的意图，并根据不同的判断结果执行相应的操作。

在XAgent/engines/interaction.py文件中，InterationJudge类的judging方法被调用。该方法根据传入的function_call和interrupt_msg参数，判断用户的意图，并根据不同的判断结果执行相应的操作。

具体代码分析如下：

- 方法签名：async def judging(self, function_call, interrupt_msg)
- 参数：
  - function_call: 一个字典，包含两个键值对，分别是"argument"和"judgement"。"argument"表示用户的输入内容，"judgement"表示对用户意图的判断结果。
  - interrupt_msg: 用户的中断消息。

- 方法内部逻辑：
  - 将用户的消息添加到交互历史记录中。
  - 使用recorder记录判断结果。
  - 根据判断结果进行不同的操作：
    - 如果判断结果为InterationJudge.exit，向用户询问是否真的要退出当前任务，并根据用户的回答执行相应的操作。
    - 如果判断结果为InterationJudge.respond，记录用户的回答。
    - 如果判断结果为InterationJudge.chat，根据用户的输入内容选择不同的聊天引擎，并获取聊天的回复。
    - 如果判断结果为InterationJudge.detail，记录用户的附加指令，并设置中断消息。
    - 如果判断结果为其他值，抛出异常。

**注意事项**：
- 在使用InterationJudge类的judging方法时，需要传入function_call和interrupt_msg参数。
- function_call参数是一个字典，包含两个键值对，分别是"argument"和"judgement"。
- interrupt_msg参数是用户的中断消息。
- 根据不同的判断结果，执行相应的操作。
- 如果判断结果为InterationJudge.exit，会向用户询问是否真的要退出当前任务，并根据用户的回答执行相应的操作。
- 如果判断结果为InterationJudge.respond，会记录用户的回答。
- 如果判断结果为InterationJudge.chat，会根据用户的输入内容选择不同的聊天引擎，并获取聊天的回复。
- 如果判断结果为InterationJudge.detail，会记录用户的附加指令，并设置中断消息。
- 如果判断结果为其他值，会抛出异常。
***
# ClassDef InterationRoute
**InterationRoute类的功能**：该类用于定义交互路由的常量。

**代码分析和描述**：
InterationRoute类是一个简单的枚举类，用于定义交互路由的常量。它包含以下三个常量：
- GPT：表示路由到GPT引擎。
- User：表示路由到用户输入。
- Downstream：表示路由到下游引擎。

这些常量用于在交互引擎的routing函数中确定消息的路由方向。根据传入的route_to参数，routing函数将根据不同的路由方向执行不同的逻辑。

**注意**：在使用InterationRoute类时，需要注意以下几点：
- 可以使用InterationRoute.GPT、InterationRoute.User和InterationRoute.Downstream来引用相应的常量。
- 如果传入的route_to参数不是上述三个常量之一，将会抛出异常。

请注意：
- 生成的文档内容中不应包含Markdown的标题和分隔符语法。
- 文档主要使用中文撰写，如果需要，可以在分析和描述中使用一些英文单词以增强文档的可读性，因为不需要将函数名或变量名翻译为目标语言。
***
