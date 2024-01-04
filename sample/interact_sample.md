# FunctionDef parse_tools
**parse_tools函数**：此函数的功能是解析指定YAML配置文件，并返回系统提示和功能列表。

此函数`parse_tools`接收一个参数`file_name`，该参数指定了要解析的YAML配置文件的名称。函数首先计算出配置文件的完整路径，然后打开并读取该文件。使用`yaml`库的`load`方法，它将文件内容加载为字典格式。函数从加载的字典中提取出系统提示（`sys_prompt`）和功能列表（`tools`），并将它们作为元组返回。

具体步骤如下：
1. 使用`os.path.join`和`os.path.dirname(__file__)`组合出配置文件的完整路径。这里的`__file__`是当前脚本的路径，`..`表示上一级目录，`XAgent/ai_functions/pure_functions`是配置文件所在的目录。
2. 使用`open`函数以只读模式打开文件，通过`yaml.load`读取YAML文件内容，指定`Loader=yaml.FullLoader`以加载所有YAML数据。
3. 从加载的YAML数据中提取`instruction`列表的第一个元素作为系统提示（`sys_prompt`），并提取`functions`作为功能列表（`tools`）。
4. 返回一个元组，包含系统提示和功能列表。

**注意**：
- 确保`file_name`参数提供的文件存在于`XAgent/ai_functions/pure_functions`目录下。
- 使用此函数时，需要确保`yaml`库已经安装在环境中，否则无法解析YAML文件。
- 返回的系统提示和功能列表将被用于后续的逻辑处理，因此调用此函数时需要处理好这两个返回值。

**输出示例**：
假设`interaction_routing_tools.yml`文件内容如下：
```yaml
instruction:
  - "这是一个系统提示示例。"
functions:
  - name: "功能1"
    description: "功能1的描述。"
  - name: "功能2"
    description: "功能2的描述。"
```
调用`parse_tools("interaction_routing_tools.yml")`将返回：
```python
("这是一个系统提示示例。", [{"name": "功能1", "description": "功能1的描述。"}, {"name": "功能2", "description": "功能2的描述。"}])
```

在项目中的调用情况：
在`sample/interact_sample.py`文件中，`parse_tools`函数被用于初始化过程中，用来解析`interaction_routing_tools.yml`和`interaction_judging_tools.yml`两个配置文件。解析得到的系统提示和功能列表被存储在`self.sys_prompt_route`、`self.functions_route`、`self.sys_prompt_judge`和`self.functions_judge`实例变量中，这些变量随后可用于指导交互路由和判断逻辑。
***
# ClassDef InteractionEngine
**InteractionEngine函数**: 这个类的功能是处理用户与系统之间的交互。

InteractionEngine类是一个用于处理用户与系统之间交互的引擎。它负责接收用户的输入，并根据输入进行相应的处理和判断。该类具有以下功能：

- 初始化：在初始化过程中，会进行一些框架的初始化操作，包括创建一个用于存储中断消息的队列、初始化一些任务相关的变量等。

- 持续检查计划状态：该函数作为一个监听器，会持续检查计划的状态，如果计划已经完成，则抛出一个`asyncio.CancelledError`异常。

- 生成GPT响应的包装器：该函数用于生成GPT的响应。它会对输入进行预处理，并将预处理后的参数传递给GPT模型，然后返回GPT的响应结果。

- 处理用户中断的入口：该函数用于处理用户的中断请求。它会判断当前计划是否正在执行，如果是，则生成一个判断请求，并将判断结果传递给`judging`函数进行处理。

- 执行GPT的判断结果：该函数根据GPT的判断结果执行相应的操作。根据判断结果的不同，可能会执行不同的操作，比如退出当前任务、不做回应、提供更多信息等。

- 执行初始任务的路由：该函数根据初始任务的路由结果执行相应的操作。根据路由结果的不同，可能会直接回复GPT的结果、向用户询问更多信息或者开始执行下游引擎。

- 用户输入：该函数用于接收用户的输入，并将输入放入中断消息队列中。

- 模拟计划引擎的执行：该函数用于模拟计划引擎的执行过程，以便进行演示和测试。

- 初始任务形成：该函数用于与用户进行初始任务的形成，包括与用户的对话和任务的路由。

- 运行：该函数是InteractionEngine类的主要入口函数，用于启动交互引擎的运行。它会不断循环执行初始任务形成和计划引擎的运行，直到用户主动取消或计划引擎完成。

**注意事项**：
- InteractionEngine类是一个用于处理用户与系统之间交互的引擎，它需要与其他组件配合使用才能实现完整的功能。
- 该类中的一些函数涉及到与外部系统的交互，比如调用GPT模型进行对话生成，因此需要确保相关的依赖已经安装和配置正确。
- 在使用该类时，需要注意正确设置和处理中断消息队列，以及根据具体的业务需求进行相应的配置和调整。

**输出示例**：
```
=== Starting a new round of conversation. ===

Please enter your task or inquiry: I would like to compare the price of Tesla model X and Y in 2023.

Direct reply from GPT: Sure, I can help you with that. The estimated price of Tesla Model X in 2023 is $80,000, and the estimated price of Tesla Model Y in 2023 is $70,000.

=== Finish Current Round of Task ===
```
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化一个对象的状态和属性。

该函数是一个对象的构造器（constructor），用于初始化对象时设置其初始状态。在这个特定的例子中，构造器定义了以下几个关键的属性和任务：

1. `self.interruptions`：一个异步队列（`asyncio.Queue`），用于存储中断事件。
2. `self.user_input_task`：用于处理用户输入的任务，初始时设置为`None`。
3. `self.plan_task`：用于处理计划任务的任务，初始时设置为`None`。
4. `self.interrupt_handler`：用于处理中断的处理器，初始时设置为`None`。
5. `self.status_checking_task`：用于检查状态的任务，初始时设置为`None`。
6. `self.task_formation_history`：一个列表，用于记录任务形成的历史。
7. `self.user_interrupt_history`：一个列表，用于记录用户中断的历史。
8. `self.plan_status`：表示计划状态的整数，初始化为0。
9. `self.current_task`：用于测试的当前任务描述字符串。

此外，构造器还初始化了与XAgent相关的属性：

1. `self.model`：设置为使用的模型名称，这里是`"gpt-4"`。
2. `self.sys_prompt_route` 和 `self.functions_route`：通过调用`parse_tools`函数，解析`"interaction_routing_tools.yml"`文件，获取系统提示路由和功能路由。
3. `self.sys_prompt_judge` 和 `self.functions_judge`：通过调用`parse_tools`函数，解析`"interaction_judging_tools.yml"`文件，获取系统提示判断和功能判断。
4. `self.function_manager`：创建一个`FunctionManager`对象，用于管理AI功能，它加载了`functions`和`pure_functions`目录下的函数。
5. `self.arguments`：通过`function_manager`获取名为`'simple_thought'`的函数的参数模式。

**注意**：
- 在使用该构造器时，需要确保相关的配置文件（如`"interaction_routing_tools.yml"`和`"interaction_judging_tools.yml"`）存在且格式正确，因为构造器中调用了`parse_tools`函数来解析这些文件。
- `FunctionManager`的初始化依赖于文件路径，因此需要确保`ai_functions`目录的结构和位置与代码中的预期相匹配。
- 由于构造器中使用了`asyncio.Queue`，这意味着涉及到该队列的操作应该在异步环境中进行。
- `self.arguments`的获取依赖于`FunctionManager`能够正确解析和加载`'simple_thought'`函数，如果该函数不存在或者有错误，可能会导致错误或异常。
## AsyncFunctionDef check_plan_status
**check_plan_status 函数**: 该函数的作用是监控计划任务的状态，并在计划任务完成时更新状态并触发取消异常。

该`check_plan_status`函数是一个异步函数，用于在执行计划任务时持续检查任务的状态。它通过无限循环来实现持续监控，并在每次循环中暂停1秒以避免过度占用CPU资源。

函数内部逻辑如下：
- 函数使用`asyncio.sleep(1)`暂停1秒，这是为了减少对CPU的占用并给其他并发任务执行的机会。
- 使用`if self.plan_status == 1:`检查成员变量`plan_status`的值是否为1，这个值表示计划任务已经开始执行。
- 如果计划任务(`self.plan_task`)还未完成(`not self.plan_task.done()`)，则继续循环监控。
- 如果计划任务已完成(`self.plan_task.done()`)，则将`plan_status`设置为0，表示计划任务已结束，并通过抛出`asyncio.CancelledError`异常来通知其他相关的异步任务。

在项目中调用`check_plan_status`函数的情况如下：
- 在`sample/interact_sample.py`文件中的`run`异步方法中，首先创建了几个异步任务，包括用户输入处理、中断处理、计划任务执行以及状态检查。
- 在启动计划任务执行后，通过`self.status_checking_task = asyncio.create_task(self.check_plan_status())`创建了一个新的异步任务来运行`check_plan_status`函数。
- 使用`await asyncio.gather(...)`等待所有任务完成。如果计划任务完成，`check_plan_status`函数将触发取消异常，导致`asyncio.gather`捕获到这个异常，并取消其他正在执行的任务。
- 在`except asyncio.CancelledError:`块中处理了取消异常，打印了相应的消息，并在`finally`块中取消了所有的异步任务。

**注意**：
- 由于`check_plan_status`函数设计为无限循环，因此在计划任务完成后必须有机制来停止这个循环。在本例中，通过抛出`asyncio.CancelledError`来实现。
- 在实际使用中，需要确保`plan_status`和`plan_task`属性在类的其他地方被正确地初始化和管理。
- 由于该函数会抛出`asyncio.CancelledError`异常，调用该函数的代码需要妥善处理这个异常，以避免程序意外退出或资源未能正确释放。
## AsyncFunctionDef generate
**generate函数**: 此函数的功能是处理用户消息和函数调用，生成并返回函数调用的结果。

详细代码分析和描述：
`generate`函数是一个异步函数，它接收三个参数：`messages`、`functions`和`func_name`。这个函数的主要作用是对输入进行预处理，然后发送用户消息，并根据返回的响应生成函数调用的结果。

1. 首先，函数遍历`self.arguments['properties']`字典，并将其键值对更新到`functions[0]['parameters']['properties']`中。如果键存在于`self.arguments['required']`列表中，那么这个键也会被添加到`functions[0]['parameters']['required']`列表中。

2. 接下来，函数调用`objgenerator.chatcompletion`方法，这是一个异步方法，它发送用户消息和函数信息，并等待响应。这个方法的参数包括`messages`（用户消息列表），`functions`（函数列表），`function_call`（包含函数名的字典），以及`self.model`（模型信息）。

3. 函数打印出从`chatcompletion`方法返回的响应，并从响应中解析出函数调用的参数。这是通过读取响应中的`choices`列表的第一个元素中的`message`字段来完成的，然后将其`function_call`字段的`arguments`解析为JSON格式。

4. 最后，函数返回解析出的函数调用参数。

在项目中的调用情况：
- 在`sample/interact_sample.py`文件中，`generate`函数被用于处理中断消息。当计划正在执行时，它会生成一个判断，并根据判断结果调用`judging`函数。
- 同样在`sample/interact_sample.py`文件中，`generate`函数还被用于初始化任务形成过程。它会与用户交互，直到任务形成完成，并返回是否开始下游任务的标志。

**注意**：
- 使用此函数时，需要确保`self.arguments`、`objgenerator`和`self.model`已经正确初始化。
- 由于此函数是异步的，调用它时需要使用`await`关键字。
- 函数调用`objgenerator.chatcompletion`可能会根据实际情况进行网络请求，因此需要考虑网络延迟和异常处理。

**输出示例**：
函数可能返回的结果示例为：
```json
{
  "judgement": "Provide_More_Information",
  "argument": "The user would like to compare Tesla model Z now."
}
```
这是一个包含判断结果和参数的字典，具体内容会根据实际情况和`chatcompletion`方法的响应而变化。
## AsyncFunctionDef handle_interrupts
**handle_interrupts函数**: 该函数的功能是处理中断事件。

该`handle_interrupts`函数是一个异步函数，它的主要作用是在执行计划（Plan）时，监听并处理中断消息。在`interact_sample.py`文件中，该函数被用于在用户输入和计划引擎执行的同时，处理可能发生的中断。

详细代码分析如下：

1. 函数通过无限循环`while True`来持续监听中断队列`self.interruptions`。
2. 当接收到中断消息`interrupt_msg`时，首先检查当前的计划状态`self.plan_status`。如果计划正在执行（状态为1），则继续处理中断；否则，如果计划已经结束，将退出循环并清空中断队列。
3. 在计划执行状态下，函数首先打印接收到的中断消息，然后构造一个消息列表`messages`，包含系统提示和用户的中断消息。
4. 接下来，函数调用`self.generate`方法，传入消息列表和判断功能的副本`self.functions_judge`，以生成判断调用`function_call`。
5. 函数进一步调用`self.judging`方法，传入生成的判断调用`function_call`，以执行具体的判断逻辑。

在`interact_sample.py`文件中，`handle_interrupts`函数被用于异步任务`self.interrupt_handler`中。在用户输入任务`self.user_input_task`和状态检查任务`self.status_checking_task`并行执行的同时，`handle_interrupts`函数负责处理任何中断事件。

**注意**：
- 由于`handle_interrupts`是一个异步函数，它需要在异步环境中被调用，并且通常与其他异步任务一起使用`asyncio.gather`来并发执行。
- 函数内部的`self.generate`方法可能需要根据实际情况进行适当的修改，以适应不同的判断逻辑和中断处理需求。
- 在实际使用中，需要确保`self.interruptions`队列被正确地初始化和管理，以便函数能够接收到中断消息。
- 当计划状态不是执行状态时，函数会打印一条消息并退出循环，这意味着中断处理将停止，直到下一次计划开始。
## AsyncFunctionDef judging
**judging函数**: 该函数的功能是根据传入的`function_call`字典中的`judgement`字段来执行不同的操作。

该`judging`函数是一个异步函数，它接收一个名为`function_call`的字典参数。这个字典包含了两个关键字段：`argument`和`judgement`。函数内部根据`judgement`字段的值来决定如何处理当前的任务或中断。

- 当`judgement`的值为`"Exit_Current_Task"`时，函数会打印一条警告信息，表示正在退出当前任务，并取消当前的任务计划（`self.plan_task.cancel()`），然后抛出`asyncio.CancelledError`异常来中断任务。
- 当`judgement`的值为`"Not_Related"`时，函数会打印一条信息，告知用户输入与当前任务无关，并且程序会暂停5秒钟（`await asyncio.sleep(5)`）。
- 当`judgement`的值为`"Provide_More_Information"`时，函数会打印出用户提供的额外信息，并表示这些信息将被用于设置中断（`set_interrupt()`）。接着程序暂停5秒钟，并且将信息设置为中断消息（`await INTERRUPT.set_message(argument)`）。
- 如果`judgement`的值不是上述任何一个，则函数会抛出一个异常，表示`judging`函数的输出不合法。

在项目中，`judging`函数被`sample/interact_sample.py`文件中的`handle_interrupts`异步函数调用。在`handle_interrupts`函数中，当计划正在执行（`self.plan_status == 1`）时，会从中断队列中获取中断消息，并根据这些消息生成一个判断（`function_call`），然后调用`judging`函数来处理这个判断。

**注意**：
- `judging`函数是异步的，因此在调用时需要使用`await`关键字。
- 在调用`judging`函数之前，需要确保`function_call`字典中包含有效的`judgement`和`argument`字段。
- `judging`函数中的`set_interrupt()`和`INTERRUPT.set_message(argument)`需要在实际项目中定义，这里没有给出具体实现，因此在使用时需要注意这两个函数或方法的实现和调用。
- 当`judging`函数抛出异常时，需要在调用它的上下文中进行异常处理，以确保程序的稳定性和正确的错误处理流程。
## AsyncFunctionDef routing
**routing函数**: 该函数的功能是根据传入的函数调用信息，决定消息的路由方向，并返回是否结束当前循环以及是否启动下游引擎的信息。

该`routing`函数是一个异步函数，它接收一个名为`function_call`的字典参数，该字典包含了执行路由所需的信息。函数内部根据`function_call`字典中的`route_to`字段来决定如何处理传入的消息，并根据不同的路由目标执行相应的操作。

- 当`route_to`的值为`"Route_GPT"`时，函数会直接打印出由GPT生成的回复，并返回一个元组`(True, False)`，表示当前循环可以结束，但不需要启动下游引擎。
- 当`route_to`的值为`"Route_User"`时，函数会提示用户输入更多信息，并将系统和用户的消息添加到`task_formation_history`列表中。然后返回`(False, False)`，表示当前循环不结束，也不启动下游引擎。
- 当`route_to`的值为`"Route_Downstream"`时，函数会打印出任务目标的总结，并告知用户将开始执行任务。同时，它会将任务目标保存到`current_task`变量中，并返回`(True, True)`，表示当前循环结束，并且需要启动下游引擎。
- 如果`route_to`的值不是上述任何一个，函数会抛出一个异常，表示路由函数的输出不合法。

在项目中，`routing`函数被`interact_sample.py`文件中的`initial_task_formation`异步函数调用。在这个调用场景中，`initial_task_formation`函数首先初始化任务形成历史记录，然后进入一个循环，不断地与用户交互，通过调用`generate`函数生成路由信息，并调用`routing`函数来处理这些信息。根据`routing`函数的返回值，决定是否结束循环以及是否启动下游引擎。

**注意**：
- 在使用`routing`函数时，需要确保传入的`function_call`字典包含`argument`和`route_to`两个关键字段。
- 由于`routing`函数可能会请求用户输入，因此在没有用户交互的环境中（如自动化测试）使用时需要特别注意。
- 当`route_to`不是预期值时，函数会抛出异常，调用方需要准备好异常处理逻辑。

**输出示例**：
假设`function_call`字典为`{"argument": "你好，世界！", "route_to": "Route_GPT"}`，则`routing`函数的输出可能为：
```
(True, False)
```
表示当前循环结束，不启动下游引擎。同时，控制台会打印出：
```
Direct reply from GPT: 你好，世界！
```
## AsyncFunctionDef user_input
**user_input函数**: 该函数的功能是接收用户在计划执行过程中的输入，并将其存储和处理。

该`user_input`函数是一个异步函数，设计用来在一个事件循环中运行，以便在其他异步任务同时进行时接收用户的输入。函数的详细分析如下：

1. 函数首先获取当前运行的事件循环对象，这是异步编程中的一个常见做法，以便在该循环中执行异步任务。

2. 函数进入一个无限循环，这意味着它将持续运行，直到外部条件触发其退出。

3. 在循环内部，函数检查成员变量`self.plan_status`的值。如果该值为1，表示当前有一个计划正在执行中，此时需要接收用户的进一步指令。

4. 使用`loop.run_in_executor`方法，函数异步地调用内置的`input`函数来获取用户输入。这里使用`None`作为执行器参数，表示使用默认的执行器，即在一个线程池中执行阻塞的`input`函数。

5. 用户的输入被追加到成员变量`self.user_interrupt_history`列表中，这个列表记录了用户的所有中断历史。

6. 最后，用户的输入被放入一个名为`self.interruptions`的异步队列中，以便其他部分的代码可以处理这些中断。

在项目中调用`user_input`函数的情况如下：

- 在`interact_sample.py`文件的`run`方法中，`user_input`函数被封装成一个异步任务，并与其他任务一起被`asyncio.gather`调用。这允许用户输入与计划引擎的执行和中断处理并行进行。

- `self.plan_status`被设置为1，以启动用户输入的接收。

- 如果用户输入任务`self.user_input_task`存在，它会在`run`方法的最后被取消，这是为了确保在一个对话轮次结束时，所有相关的异步任务都能被适当地清理。

**注意**：
- 由于`input`函数是阻塞的，所以在异步环境中使用`run_in_executor`来避免阻塞事件循环。
- 在使用该函数时，需要确保`self.plan_status`、`self.user_interrupt_history`和`self.interruptions`这些成员变量已经被正确初始化。
- 由于该函数设计为无限循环，应当在适当的时机从外部取消这个异步任务，以避免无限等待用户输入导致的资源泄露。
## AsyncFunctionDef run_plan_engine
**run_plan_engine 函数**: 该函数的功能是模拟计划引擎的执行过程，用于演示目的。

该函数`run_plan_engine`是一个异步函数，它接收一个参数`task`，这个参数代表了要执行的任务。函数的主要作用是模拟计划引擎（plan engine）在执行一个任务时的行为。

在函数体内，使用了`asyncio.sleep(1000)`这个异步操作来模拟任务执行的时间。这里的`1000`表示函数将暂停执行1000秒，这个时间长度在实际应用中通常是不切实际的，因此我们可以认为这里的值是为了演示目的而故意设置得很长。

由于`asyncio.sleep`是一个异步函数，它不会阻塞整个程序的运行，而是在模拟执行期间释放控制权，允许其他异步任务在这段时间内运行。这是异步编程的一个重要特性，它可以提高程序在处理I/O操作或其他等待操作时的效率。

**注意**：
- 在使用`run_plan_engine`函数时，需要确保你的程序运行环境支持异步编程，即在一个支持异步操作的事件循环中调用此函数。
- 由于该函数是为了演示而设计的，所以在实际应用中，你需要根据实际任务执行所需的时间来调整`asyncio.sleep`中的等待时间。
- 调用这个函数时，需要使用`await`关键字，因为它是一个异步函数。
- 由于该函数没有返回值，它仅仅用于模拟执行过程，因此不会对`task`参数进行任何操作或返回执行结果。
## AsyncFunctionDef initial_task_formation
**initial_task_formation函数**: 该函数的功能是初始化任务形成过程。

该`initial_task_formation`函数是一个异步函数，它的主要作用是与用户进行交互，以形成一个初始的任务。在这个过程中，它会记录系统和用户的交互历史，并根据用户的输入来决定是否需要启动下游的任务处理引擎。

函数执行流程如下：
1. 首先，函数会创建一个包含系统提示信息的`Message`对象，并将其原始数据形式添加到`task_formation_history`列表中。
2. 然后，函数会提示用户输入任务或查询，并将用户的输入同样以`Message`对象的形式添加到`task_formation_history`列表中。
3. 接下来，函数进入一个循环，循环体中会调用`generate`函数生成一个函数调用，并通过`routing`函数来处理这个调用。如果`routing`函数返回表示完成的信号，则退出循环。
4. 最后，函数返回一个标志，指示是否需要启动下游的任务处理引擎。

在项目中，`initial_task_formation`函数被`interact_sample.py`文件中的`run`异步方法调用。在`run`方法中，首先会调用`initial_task_formation`函数来与用户进行交互并形成任务。如果确定需要启动下游引擎，则会创建并运行计划引擎和执行引擎，并处理用户输入和中断。

**注意**：
- 由于`initial_task_formation`函数是异步的，调用它时需要使用`await`关键字。
- 函数中使用了`input`函数来获取用户输入，这意味着它会阻塞当前的事件循环直到用户输入完成。在异步环境中，可能需要使用异步的输入方法来避免阻塞。
- 函数内部使用了`asyncio.gather`来并发执行多个任务，这要求调用者在一个支持异步操作的环境中运行它。

**输出示例**：
函数返回的`start_downstream`可能是一个布尔值，表示是否需要启动下游的任务处理引擎。例如，如果用户的查询已经被GPT直接回答，那么就没有必要启动下游引擎，此时`start_downstream`可能是`False`。如果需要进一步处理，`start_downstream`可能是`True`。
## AsyncFunctionDef run
**run函数**: 该函数的功能是执行一个异步的对话循环，处理用户输入、任务规划、执行引擎运行以及中断处理。

该`run`函数是一个异步函数，它定义在一个类中（尽管代码片段中没有显示类的定义），并且它的目的是在一个无限循环中处理一系列的对话任务。以下是对代码的详细分析：

1. 函数开始时，会打印一条消息表示开始了新一轮的对话。

2. 使用`try`语句块来捕获可能发生的异常，以便于优雅地处理错误和中断。

3. 调用`self.initial_task_formation()`方法来初始化任务。这个方法可能是异步的，因此使用`await`来等待它的完成。如果这个方法返回`False`，表示GPT已经回答了查询，那么就跳过下游引擎的启动，继续下一轮循环。

4. 如果需要启动下游引擎，则创建`PlanEngine`和`ReActEngine`实例，这些可能是规划和执行的引擎。

5. 创建一个`PlanExecutionNode`实例，它代表了当前任务的初始节点。

6. 使用`asyncio.create_task`来异步启动规划引擎，同时传递初始节点和执行引擎。

7. 设置`self.plan_status`为1，表示规划引擎已经接收到用户输入并开始规划。

8. 启动中断处理器`self.interrupt_handler`，这是一个异步任务，用于处理可能发生的中断。

9. 启动用户输入任务`self.user_input_task`，允许用户在规划引擎执行和中断处理的同时输入。

10. 启动状态检查任务`self.status_checking_task`，用于检查规划状态。

11. 使用`asyncio.gather`等待上述所有任务完成。这意味着函数会在所有任务完成或被取消之前一直等待。

12. 如果`asyncio.CancelledError`异常被抛出，表示规划引擎的执行被取消了。

13. 在`finally`块中，取消所有可能仍在运行的任务，并打印消息表示当前任务轮次结束。

14. 最后，函数等待3秒钟，可能是为了在轮次之间提供一个短暂的暂停。

**注意**:
- 由于这是一个异步函数，调用它时需要使用`await`关键字，或者在另一个异步函数中使用`asyncio.create_task`来运行它。
- 函数中使用了多个`asyncio.create_task`来并行运行多个异步任务，这要求调用者了解`asyncio`库的使用。
- 该函数可能是设计用于一个聊天机器人或者类似的交互式应用程序，其中涉及到用户输入、任务规划和执行、以及中断处理。
- 函数中提到的`CONFIG`变量没有在代码片段中定义，它应该是在类的其他部分或者全局配置中定义的。
- 函数中的`self.current_task`、`self.plan_task`、`self.user_input_task`、`self.interrupt_handler`和`self.status_checking_task`等属性应该在类的其他部分定义和初始化。
***
