# ClassDef PlanAgent
**PlanAgent功能**：此类的功能是生成和校正计划。

PlanAgent是一个继承自BaseAgent的代理类，它具备生成和校正计划的能力。在XAgent系统中，PlanAgent用于处理与计划生成相关的任务，例如根据用户的查询生成一个执行计划，或者对现有的计划进行优化和调整。

**详细代码分析**：
- `abilities`属性定义了PlanAgent所需的能力集合，包括计划生成（plan_generation）和计划细化（plan_refinement）。
- `__init__`方法是PlanAgent的构造函数，它接受一个XAgentConfig配置对象作为参数。构造函数调用基类BaseAgent的构造函数，并设置了特定的配置文件（profile）、步骤提示（step_prompt）和聊天提示（chat_prompt）。此外，它还初始化了一个初始提示（inital_prompt）。
- `get_step_message`是一个异步方法，根据不同的模式（mode）返回下一步的消息。如果模式是"generate"，它将返回一个格式化后的初始提示；否则，它将调用基类的同名方法。
- `generate_plan`是一个异步方法，用于根据用户的查询（query）和工具选择（tool_choice）生成一个执行计划。该方法通过调用`step`方法执行一个步骤，并从返回的工具调用中提取任务列表，将其转换为Task对象列表。

**在项目中的调用情况**：
- 在`XAgent/agent/plan.py`文件中，PlanAgent类被实例化，并传入了配置对象。
- 在`XAgent/core.py`文件中，PlanAgent类被添加到了可用代理列表中，这意味着XAgent系统可以根据需要创建PlanAgent的实例。
- 在`XAgent/engines/plan.py`文件中，PlanAgent类被用作引擎的一部分，用于处理计划生成和校正的逻辑。

**注意**：
- 使用PlanAgent时，需要确保传入正确的XAgentConfig配置对象，因为它会影响PlanAgent的行为和性能。
- PlanAgent的方法大多是异步的，需要在异步环境中调用。

**输出示例**：
```python
# 假设generate_plan方法被调用，并返回了一个任务列表
tasks = await plan_agent.generate_plan(query="我该如何学习Python？", tool_choice={})
# tasks可能是这样的：
[
    Task(name="打开教程网站", ...),
    Task(name="阅读基础语法部分", ...),
    Task(name="完成一个小项目", ...)
]
```
在这个示例中，`generate_plan`方法返回了一个包含多个Task对象的列表，每个Task对象代表了学习Python的一个步骤。
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化 PlanAgent 类的实例。

该函数是 PlanAgent 类的构造函数，用于创建 PlanAgent 对象时初始化其状态。构造函数接受一个参数 `config`，该参数是一个 `XAgentConfig` 类型的对象，包含了执行计划所需的配置信息。

在函数内部，首先通过 `super(PlanAgent, self).__init__` 调用基类的构造函数。这里的基类是通过继承得到的，而 `super()` 函数是用来调用父类（超类）的一个方法。在这个调用中，传递了几个关键的参数：

- `config`：传递给基类构造函数的配置对象，它包含了执行计划时所需的各种配置信息。
- `profile`：这是一个格式化的字符串，其中包含了最大计划树宽度（`max_plan_tree_width`）和最大计划树深度（`max_plan_tree_depth`），这两个值是从传入的 `config` 对象的 `execution` 属性中获取的。
- `step_prompt` 和 `chat_prompt`：这两个参数是预定义的提示字符串，用于在执行计划时与用户交互。

构造函数还设置了一个实例变量 `self.inital_prompt`，它的值被初始化为 `INIT`。这个变量可能用于在计划的执行过程中提供初始的提示信息。

**注意**：在使用这段代码时，需要确保传入的 `config` 对象具有有效的配置信息，特别是 `execution.max_plan_tree_width` 和 `execution.max_plan_tree_depth` 属性，因为这些属性将直接影响计划执行的行为。此外，`INIT` 应该是一个定义好的字符串常量，用于初始化提示信息。
## AsyncFunctionDef get_step_message
**get_step_message函数**: 此函数的功能是返回下一步的消息。

`get_step_message`函数是一个异步函数，它的作用是根据给定的模式（mode）返回下一步操作的提示信息。该函数接受一个名为`mode`的参数，以及可变关键字参数`**kwargs`。

参数`mode`是一个字面量类型，它有三个可能的值："default"、"conclusion"和"generate"。默认值为"default"。

- 当`mode`参数的值为"generate"时，函数会使用`deepcopy`函数来复制`self.inital_prompt`属性（初始提示信息），并使用`format`方法将`**kwargs`中的关键字参数替换到该提示信息中，然后返回这个经过格式化的字符串。
- 当`mode`参数的值为"default"或"conclusion"时，函数会调用父类的`get_step_message`方法，并将`mode`参数传递给它，然后返回父类方法的结果。

由于这是一个异步函数，因此在调用时需要使用`await`关键字。

**注意**：
- 在使用`get_step_message`函数时，需要确保传递给`format`方法的关键字参数与`self.inital_prompt`字符串中的占位符相匹配，否则可能会抛出`KeyError`异常。
- 由于函数可能会调用父类的方法，因此在使用时需要确保父类中也实现了`get_step_message`方法。

**输出示例**：
假设`self.inital_prompt`的值为"Hello, {name}! How can I assist you today?"，并且调用函数时传递了`mode="generate"`和`kwargs={'name': 'Alice'}`，那么函数的返回值可能会是：
```
"Hello, Alice! How can I assist you today?"
```
如果`mode`参数的值为"default"，则函数的返回值将依赖于父类`get_step_message`方法的实现和返回结果。
## AsyncFunctionDef generate_plan
**generate_plan 函数**: 此函数的功能是生成查询计划。

generate_plan 函数是一个异步函数，它接收一个查询字符串 `query` 和一个工具选择字典 `tool_choice`，然后返回一个任务列表 `list[Task]`。这个函数的主要作用是根据给定的查询和工具选择，生成一个执行计划，该计划由多个子任务组成。

在函数内部，首先调用 `self.step` 方法执行步骤，传入步骤参数（包括模式 "generate" 和查询字符串 `query`），以及工具选择 `tool_choice`。`self.step` 方法的执行结果会返回两个值：`replies` 和 `tool_calls`。在这个上下文中，我们主要关注 `tool_calls`，因为它包含了生成的工具调用信息。

`tool_calls` 是一个列表，但在这个函数中，我们只关注列表中的第一个元素 `tool_call`。`tool_call` 包含了生成的子任务信息，这些信息存储在 `tool_call.arguments["subtasks"]` 中。每个子任务都是一个字典，包含了创建 `Task` 对象所需的信息。

函数最后通过列表推导式，将 `tool_call.arguments["subtasks"]` 中的每个子任务字典转换成 `Task` 对象，并返回这个任务列表。

在项目中，`generate_plan` 函数被 `XAgent/engines/plan.py` 文件中的 `run` 方法调用。在 `run` 方法中，它用于生成初始计划，即根据给定的任务目标 `task.goal`，生成一系列的子任务。这些子任务随后被添加到执行跟踪图 `self.exec_track` 中，作为执行引擎处理的一部分。

**注意**：
- 由于 `generate_plan` 是一个异步函数，调用它时需要使用 `await` 关键字。
- 函数返回的任务列表是 `Task` 对象的列表，因此调用方需要了解 `Task` 类的结构和用法。
- 在调用此函数时，需要确保 `tool_choice` 字典格式正确，以便函数能够根据提供的工具选择生成正确的计划。

**输出示例**：
假设 `tool_call.arguments["subtasks"]` 返回以下数据：
```python
[
    {"name": "subtask1", "type": "TaskTypeA", "details": "Do something"},
    {"name": "subtask2", "type": "TaskTypeB", "details": "Do something else"}
]
```
那么 `generate_plan` 函数将返回如下的 `Task` 对象列表：
```python
[
    Task(name="subtask1", type="TaskTypeA", details="Do something"),
    Task(name="subtask2", type="TaskTypeB", details="Do something else")
]
```
***
