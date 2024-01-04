# ClassDef BaseThreadAgent
**BaseThreadAgent函数**：这个类的功能是实现基本的线程代理。

BaseThreadAgent是一个基本的线程代理类，用于处理线程的创建、初始化、步骤执行和工具调用等操作。它是其他线程代理类的基类，提供了一些通用的方法和属性。

该类的构造函数`__init__`接受四个参数：`config`、`system_prompt`、`task_prompt_template`和`thread_id`。其中，`config`是配置信息，`system_prompt`是系统提示信息，`task_prompt_template`是任务提示模板，`thread_id`是线程的ID。在构造函数中，将这些参数赋值给对应的属性。

BaseThreadAgent类还定义了`__del__`方法，用于在对象被销毁时取消运行中的线程。如果运行状态不是"expired"、"completed"、"failed"或"cancelled"，则调用`asyncio.run_coroutine_threadsafe`方法取消运行中的线程。

`lazy_init`方法用于延迟初始化线程。它首先使用`client.beta.assistants.create`方法创建一个助手对象，然后根据传入的`thread_id`参数决定是创建新线程还是检索已有线程。

`init_step`方法用于初始化步骤相关的属性，将`steps`和`steps_messages`属性清空。

`run_old`方法用于执行旧版本的运行步骤。它接受多个参数，包括占位符、参数、工具、工具选择、停止标志、额外的消息等。在方法内部，根据传入的参数生成任务消息，并将其与额外的消息合并为完整的消息列表。然后调用`step`方法执行步骤。

`step`方法用于执行运行步骤。它接受消息列表和工具列表作为参数。在方法内部，首先将消息发送到线程中，然后初始化步骤相关的属性。接着使用`client.beta.threads.runs.create`方法创建一个运行对象，并将其赋值给`run`属性。然后通过循环调用`retrieve`方法来获取运行状态，直到状态为"expired"、"completed"、"failed"或"cancelled"为止。在每次循环中，如果状态发生变化或上一次检索到的步骤消息不为空，则返回当前状态。否则，等待1秒后继续循环。

`generate_task_message`方法用于生成任务消息。它接受关键字参数，并根据模板替换占位符生成任务消息。

`convert_message`方法用于将线程消息转换为消息对象。它遍历线程消息的内容，将文本类型的内容拼接为字符串，并返回消息对象。

`retrieve_message`方法用于检索消息。它接受消息ID列表作为参数，如果列表为空，则使用`client.beta.threads.messages.list`方法获取线程中的所有消息；否则，根据消息ID使用`client.beta.threads.messages.retrieve`方法获取指定的消息。最后，将获取到的消息转换为消息对象并返回。

`retrieve_tool_calls`方法用于检索工具调用。它通过检查运行对象的`required_action`属性来获取工具调用列表。如果`required_action`为空，则打印警告信息并返回空列表。否则，将工具调用信息封装为工具调用对象并返回。

`submit_tool_outputs`方法用于提交工具输出。它接受工具输出列表作为参数，并根据运行状态判断是否可以提交工具输出。如果运行状态不是"requires_action"，则打印警告信息并返回当前运行对象。否则，使用`client.beta.threads.runs.submit_tool_outputs`方法提交工具输出，并返回更新后的运行对象。

`retrieve`方法用于检索运行状态和步骤信息。它首先使用`client.beta.threads.runs.retrieve`方法获取最新的运行对象，并将其赋值给`run`属性。然后使用`client.beta.threads.runs.steps.list`方法获取运行的步骤列表，并将步骤信息存储到`steps`和`steps_messages`属性中。同时，将最后一个步骤赋值给`after`属性，以便下一次检索时获取更新的步骤信息。最后，返回运行状态。

`cancel`方法用于取消运行中的线程。它使用`client.beta.threads.runs.cancel`方法取消运行，并返回更新后的运行对象。

**注意**：使用该类时需要注意以下几点：
- 在创建BaseThreadAgent对象时，需要传入配置信息、系统提示信息和任务提示模板。
- 在执行步骤时，需要传入消息列表和工具列表。
- 可以通过调用`lazy_init`方法延迟初始化线程。
- 可以通过调用`run_old`方法执行旧版本的运行步骤。
- 可以通过调用`submit_tool_outputs`方法提交工具输出。
- 可以通过调用`retrieve`方法检索运行状态和步骤信息。
- 可以通过调用`cancel`方法取消运行中的线程。

**输出示例**：
```python
agent = BaseThreadAgent(config, system_prompt, task_prompt_template, thread_id)
agent.lazy_init(thread_id)
agent.run_old(placeholders, arguments, tools, tool_choice, stop, additional_messages)
for status in agent.step(messages, tools):
    print(status)
agent.submit_tool_outputs(tool_outputs)
agent.retrieve()
agent.cancel()
```
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化一个线程对象。

该函数是一个初始化方法，用于创建一个新的线程对象，并设置其基本属性。具体来说，该函数执行以下操作：

1. 接收配置信息（`config`），系统提示信息（`system_prompt`），任务提示模板（`task_prompt_template`），以及可选的线程ID（`thread_id`）作为参数。
2. 将传入的`config`配置信息赋值给对象的`config`属性。
3. 将传入的系统提示信息`system_prompt`赋值给对象的`system_prompt`属性。
4. 将传入的任务提示模板`task_prompt_template`赋值给对象的`task_prompt_template`属性。
5. 通过`objgenerator.client`创建一个客户端实例，并将其赋值给对象的`client`属性。
6. 初始化`assistant`、`thread`、`run`属性为`None`，这些属性可能会在后续的方法中被赋予具体的对象或值。
7. 初始化`steps`、`steps_messages`、`last_retrieved_steps`、`last_steps_messages`四个字典属性，这些字典将用于存储与线程执行相关的步骤和消息信息。其中，`steps`和`last_retrieved_steps`字典的键为步骤的字符串标识，值为`RunStep`对象；`steps_messages`和`last_steps_messages`字典的键为消息的字符串标识，值为`Message`对象。
8. 初始化`after`属性为`None`，该属性可能会在后续的方法中被赋予具体的值。

**注意**：
- 在使用该初始化函数时，需要确保传入的参数类型和结构符合预期，特别是`config`参数，它通常包含了线程运行所需的配置信息。
- `objgenerator.client`应该是在其他地方定义的一个对象生成器，用于创建客户端实例。在使用前需要确保该对象生成器已经正确初始化。
- `RunStep`和`Message`应该是在其他地方定义的类，代表执行步骤和消息，需要在使用前了解它们的结构和用法。
- 由于`thread_id`是一个可选参数，如果在创建线程对象时没有提供，那么线程的`thread_id`属性将默认为`None`。
## FunctionDef __del__
**__del__ 函数**: 该函数的功能是在对象被销毁时取消尚未完成的线程运行。

当一个对象的引用计数降到0时，Python解释器会调用对象的`__del__`方法，以执行对象销毁前的清理工作。在这段代码中，`__del__`方法被用于检查与当前对象关联的线程运行状态。如果该线程运行的状态不是“expired”（过期）、“completed”（完成）、“failed”（失败）或“cancelled”（取消）中的任何一个，那么这个方法会尝试取消该线程的运行。

具体来说，代码执行以下步骤：
1. 检查`self.run.status`，即当前对象的运行状态是否不在指定的状态列表中。
2. 如果不在，那么通过`self.client.beta.threads.runs.cancel`方法发起一个取消线程运行的请求。这个请求需要提供`run_id`和`thread_id`作为参数，分别代表运行ID和线程ID。
3. 使用`asyncio.run_coroutine_threadsafe`函数确保异步取消操作能够在正确的事件循环中执行。这里使用`asyncio.get_running_loop()`来获取当前正在运行的事件循环。

**注意**：
- `__del__`方法通常用于资源释放和清理工作，但需要注意的是，由于Python的垃圾回收机制，`__del__`方法的调用时间是不确定的，因此不建议在`__del__`方法中放置重要的或者必须执行的代码。
- 在多线程或异步编程环境中，使用`__del__`方法进行资源清理需要格外小心，以确保不会因为事件循环的问题导致资源未能正确释放。
- `asyncio.run_coroutine_threadsafe`是一个线程安全的函数，用于在事件循环中安排协程的执行，但它需要在已经运行的事件循环中调用。如果事件循环未运行或不存在，这个函数将会抛出异常。
- 在异步环境中，通常建议使用异步上下文管理器（例如`async with`语句）来处理资源的分配和释放，这样可以更加清晰地控制资源的生命周期。
## AsyncFunctionDef lazy_init
**lazy_init 函数**: 该函数的功能是异步初始化一个代理线程，并创建一个助手实例。

该`lazy_init`函数是一个异步函数，用于初始化XAgent代理的线程和助手实例。它接受一个可选的参数`thread_id`，该参数是一个字符串，用于指定要检索或创建的线程的ID。

函数的主要步骤如下：

1. 首先，函数使用`self.client.beta.assistants.create`方法创建一个新的助手实例。在创建助手时，它指定了模型为`gpt-4-1106-preview`，描述为`XAgent Assistant`，并提供了系统提示`self.system_prompt`。

2. 接下来，函数检查是否提供了`thread_id`参数：
   - 如果没有提供`thread_id`（即`thread_id`为`None`），函数将使用`self.client.beta.threads.create`方法创建一个新的线程实例。在创建线程时，它使用`self.config.default_completion_kwargs['request_timeout']`作为超时设置。
   - 如果提供了`thread_id`，函数将使用`self.client.beta.threads.retrieve`方法检索一个已存在的线程实例。在检索线程时，同样使用`self.config.default_completion_kwargs['request_timeout']`作为超时设置。

在这个过程中，`self.assistant`和`self.thread`是XAgent代理的两个关键属性，分别用于存储助手实例和线程实例。

**注意**：
- 由于`lazy_init`是一个异步函数，调用它时需要使用`await`关键字。
- 在使用`lazy_init`函数之前，确保已经有了有效的客户端实例`self.client`，并且该客户端已经配置好了相应的认证信息。
- `self.config.default_completion_kwargs['request_timeout']`是从配置中获取的默认请求超时时间，确保在调用`lazy_init`之前已经正确设置了这个配置项。
- 在实际部署时，需要确保所使用的模型`gpt-4-1106-preview`是可用的，并且与你的应用场景相匹配。
- 如果在创建线程或助手时发生错误，函数可能会抛出异常，调用者需要妥善处理这些异常情况。
## FunctionDef init_step
**init_step 函数**: 该函数的功能是初始化步骤和步骤消息。

init_step 函数是在 XAgent 项目的 agent/thread/base.py 文件中定义的一个方法。该方法的主要作用是初始化两个字典属性：steps 和 steps_messages。这两个属性分别用于存储与线程相关的步骤信息和步骤消息。

在 init_step 方法中，self.steps 和 self.steps_messages 被赋值为空字典。这意味着，每当调用 init_step 方法时，之前存储的步骤信息和步骤消息都会被清空，以便开始新的步骤记录。

在项目中，init_step 方法被调用的场景如下：

在 XAgent/agent/thread/base.py 文件中的异步方法 step 中，init_step 被调用。step 方法处理线程中的消息，并创建线程运行。在发送消息并创建线程运行之后，调用 init_step 方法来初始化步骤状态，确保后续的步骤记录是基于最新的线程状态。随后，step 方法进入一个循环，不断检查线程的状态，并在状态发生变化时，通过 yield 关键字返回新的状态。

**注意**：
- 在使用 init_step 方法时，需要确保它在步骤记录开始之前被调用，以避免覆盖已有的步骤信息。
- 由于 init_step 方法会清空步骤信息，因此在调用该方法后，任何之前存储的步骤状态都将不再可用。
- 在 step 方法中，init_step 方法的调用是在消息发送和线程运行创建之后，这有助于确保每次线程运行都是从一个干净的状态开始。
## FunctionDef run_old
**run_old 函数**: 此函数的功能是执行一个旧版本的运行逻辑。

run_old 函数是一个用于执行任务的方法。它接收多个参数，包括占位符、参数、工具、工具选择、停止条件、额外消息以及其他任意参数和关键字参数。函数的主要目的是生成一个任务消息，并将其与任何额外的消息合并，然后执行一个步骤（step）操作。

详细参数解析如下：
- `placeholders`: 一个字典，包含了任务消息生成时所需的占位符信息。
- `arguments`: 一个字典，包含了任务执行时所需的参数，如果没有提供，则默认为 None。
- `tools`: 可选参数，用于指定执行任务时可用的工具集。
- `tool_choice`: 可选参数，用于指定选择哪个工具执行任务。
- `stop`: 可选参数，用于指定停止条件。
- `additional_messages`: 一个 Message 类型的列表，包含了除了任务消息之外需要一起处理的额外消息。
- `additional_insert_index`: 一个整数，指定额外消息插入的位置，默认为 -1，表示追加到列表末尾。
- `*args`: 用于接收任意数量的位置参数。
- `**kwargs`: 用于接收任意数量的关键字参数。

函数首先调用 `generate_task_message` 方法，使用 `placeholders` 中的信息生成任务消息。然后，将这个任务消息与 `additional_messages` 列表中的消息合并，形成一个完整的消息列表 `full_messages`。最后，调用 `step` 方法，将 `full_messages` 和 `tools` 作为参数传入，执行任务的下一步骤。

**注意**：
- 确保在调用此函数时提供正确的 `placeholders` 和 `arguments`，以便正确生成和执行任务。
- 如果需要在特定位置插入额外消息，可以通过 `additional_insert_index` 参数来指定。
- 此函数可能是为了向后兼容而保留的旧逻辑，因此在新的代码中可能不推荐使用。

**输出示例**：
由于 `run_old` 函数的输出取决于 `step` 方法的实现，因此没有一个固定的输出示例。但是，可以预期的是，它会返回 `step` 方法的执行结果，这可能是一个表示任务执行状态的对象或者其他类型的数据。
## AsyncFunctionDef step
**step 函数**: 该函数的功能是在一个线程中执行步骤，并在执行过程中产生状态更新。

该`step`函数是一个异步函数，它在`XAgent`项目中的`agent/thread/base.py`文件中定义。它的主要作用是在一个线程（thread）中执行一系列的步骤，并且在执行的过程中，能够不断地产生状态更新，供调用者根据状态做出相应的处理。

函数接收两个参数：
- `messages`：一个`Message`对象列表，表示需要在线程中创建的消息。
- `tools`：一个包含工具信息的字典列表，这些工具将会在线程的运行中被使用。

函数的执行流程如下：
1. 遍历`messages`列表中的每个`Message`对象，并使用`self.client.beta.threads.messages.create`方法创建线程消息。
2. 调用`self.init_step()`方法来初始化步骤。
3. 使用`self.client.beta.threads.runs.create`方法设置线程的运行环境，包括线程ID、助手ID和工具列表。
4. 调用`self.retrieve()`方法获取当前线程的状态。
5. 使用一个`while`循环来不断检查线程的状态，直到状态变为"expired"（过期）、"completed"（完成）、"failed"（失败）或"cancelled"（取消）之一。
6. 如果状态发生变化或者`self.last_steps_messages`列表中有新的消息，则产生一个新的状态更新，并使用`yield`语句返回给调用者。
7. 如果状态没有变化且没有新的消息，则暂停1秒钟后继续检查状态。

在项目中，`step`函数被`run_old`方法调用。`run_old`方法生成一个任务消息，并可能添加额外的消息，然后调用`step`函数来执行步骤。

**注意**：
- 由于`step`函数是异步的，调用它时需要使用`await`关键字，或者在其他异步函数中使用。
- `yield`关键字的使用意味着`step`函数是一个异步生成器，调用者需要使用异步迭代来获取所有的状态更新。
- 在调用`step`函数时，需要确保传入的`messages`和`tools`参数格式正确，否则可能会导致线程创建或运行失败。
- 函数中的`self.client`、`self.thread`和`self.assistant`是在类的其他地方定义的实例属性，需要确保在调用`step`函数之前这些属性已经被正确初始化。
## FunctionDef generate_task_message
**generate_task_message 函数**: 该函数的功能是生成任务消息。

该函数`generate_task_message`是一个用于生成任务消息的方法。它接收任意数量的关键字参数（`**kwargs`），并将这些参数用于替换任务提示模板中的占位符。任务提示模板是一个字符串，其中包含形如`{{placeholder}}`的占位符，这些占位符将被实际的参数值所替换。

函数的工作流程如下：
1. 首先，函数通过`deepcopy`函数复制了一个任务提示模板，以确保原始模板不会被修改。
2. 然后，函数遍历传入的关键字参数，对于每一对键值对，函数将模板中的`{{key}}`占位符替换为相应的`value`值。
3. 最后，函数创建了一个`Message`对象，该对象包含了替换后的任务消息内容，并将角色设置为`"user"`。

在项目中，`generate_task_message`函数被`run_old`方法调用。在`run_old`方法中，`placeholders`字典被用作`generate_task_message`函数的参数，生成任务消息。然后，这个任务消息被添加到一个消息列表中，该列表可能还包含其他额外的消息，并被传递给`step`方法进行进一步处理。

**注意**：
- 确保传入的关键字参数与任务提示模板中的占位符相匹配，否则占位符将不会被替换。
- 使用`deepcopy`是为了避免修改原始模板，这在模板需要被重复使用时非常重要。

**输出示例**：
假设任务提示模板为 `"Hello, {{name}}! Your task is to {{task}}."`，并且调用`generate_task_message(name="Alice", task="complete the report")`，则返回的`Message`对象可能如下所示：

```python
Message(
    role="user",
    content="Hello, Alice! Your task is to complete the report."
)
```
## FunctionDef convert_message
**convert_message 函数**: 此函数的功能是将 ThreadMessage 对象转换为 Message 对象。

convert_message 函数是一个用于处理消息转换的方法。它接收一个 ThreadMessage 类型的参数，并返回一个 Message 类型的对象。ThreadMessage 是一个包含多个消息内容项的对象，而 Message 则是一个简化的消息表示，主要用于后续处理。

在 convert_message 函数内部，首先定义了一个空字符串 content，用于存储转换后的消息文本内容。随后，函数遍历 ThreadMessage 对象中的 content 属性，该属性是一个包含多个消息内容项的列表。对于列表中的每个项，函数检查其类型是否为 "text"。如果是，那么将该文本内容项的 text 属性的 value 值追加到 content 字符串中，并在每个文本内容后添加一个换行符。

最后，函数创建并返回一个新的 Message 对象，将原 ThreadMessage 对象的 role 属性值赋给新对象的 role 属性，将拼接好的文本内容 content 赋给新对象的 content 属性。

在项目中，convert_message 函数被以下几个场景调用：

1. 在 retrieve_message 异步方法中，用于从线程中获取一系列消息，并将每个消息通过 convert_message 函数转换为 Message 对象后，添加到一个列表中返回。

2. 在 retrieve 异步方法中，用于获取线程中的步骤信息。当步骤类型为 "message_creation" 且状态为 "completed" 时，会通过 convert_message 函数将步骤中的消息转换为 Message 对象，并存储于 last_steps_messages 字典中。

**注意**：在使用 convert_message 函数时，需要确保传入的 ThreadMessage 对象包含有效的 content 属性，且该属性中的项具有正确的 type 和 text 属性，以便函数能正确执行转换。

**输出示例**：
假设有一个 ThreadMessage 对象，其 content 属性包含两个文本内容项，分别为 "Hello" 和 "World"。调用 convert_message 函数后，可能得到的 Message 对象的 content 属性将是：

```
Hello
World
```

其中，每个文本内容项之间用换行符隔开。
## AsyncFunctionDef retrieve_message
**retrieve_message 函数**: 此函数的功能是检索并返回一系列消息对象。

此函数是一个异步函数，用于从一个线程中检索消息。它接受一个可选的参数 `message_ids`，这是一个字符串列表，表示要检索的消息的ID。如果提供了 `message_ids`，函数将只检索列表中指定的消息；如果没有提供，它将检索与当前线程ID关联的所有消息。

函数的详细分析如下：

1. 函数定义为异步，这意味着它需要在异步环境中运行，并且在调用时需要使用 `await` 关键字。

2. 函数接受一个名为 `message_ids` 的参数，它是一个字符串列表，默认值为 `None`。这个参数指定了要检索的消息ID。

3. 函数内部首先创建了一个空列表 `messages`，用于存储检索到的消息对象。

4. 如果没有提供 `message_ids` 参数（即它为 `None`），函数将使用异步for循环来遍历当前线程中的所有消息。对于每条消息，它会调用 `self.convert_message` 方法将消息转换为 `Message` 对象，并将其添加到 `messages` 列表中。

5. 如果提供了 `message_ids` 参数，函数将遍历这个列表，对于每个 `message_id`，它会使用 `await` 异步调用 `self.client.beta.threads.messages.retrieve` 方法来检索对应的消息，并同样使用 `self.convert_message` 方法进行转换，然后添加到 `messages` 列表中。

6. 最后，函数返回填充了 `Message` 对象的 `messages` 列表。

**注意**：
- 在使用此函数时，需要确保它被调用在一个支持异步操作的环境中。
- `self.client.beta.threads.messages.list` 和 `self.client.beta.threads.messages.retrieve` 是假定存在的API调用，实际使用时需要替换为正确的API端点。
- `self.convert_message` 是一个假定存在的方法，用于将API返回的消息格式转换为 `Message` 对象，具体实现细节需要根据实际情况定义。

**输出示例**：
```python
[
    Message(id="123", content="Hello, World!", timestamp="2021-01-01T00:00:00Z"),
    Message(id="456", content="Another message", timestamp="2021-01-01T01:00:00Z")
]
```
以上示例展示了可能的返回值，其中 `Message` 是一个假定的对象类，包含了消息的ID、内容和时间戳等属性。实际的 `Message` 对象应根据项目中定义的模型来确定。
## AsyncFunctionDef retrieve_tool_calls
**retrieve_tool_calls函数**: 该函数的功能是检索并返回当前运行中所需的工具调用列表。

该函数`retrieve_tool_calls`是一个异步函数，它返回一个包含`ToolCall`对象的列表。`ToolCall`是一个自定义对象，通常包含工具调用的相关信息，如工具的ID、名称和参数。

函数执行流程如下：

1. 首先，函数初始化一个空的列表`tool_calls`，用于存储检索到的工具调用信息。

2. 接着，函数检查`self.run.required_action`是否为`None`。如果是，表示在当前运行中没有找到工具调用，函数将通过`recorder.log`记录一条警告信息，并返回空的`tool_calls`列表。这里的`recorder.log`函数用于记录日志信息，`title_color=Fore.YELLOW`表示日志标题将以黄色显示，以提醒用户注意。

3. 如果存在必要的动作（`required_action`），函数将遍历`self.run.required_action.submit_tool_outputs.tool_calls`中的每个工具调用。

4. 对于每个工具调用，函数创建一个新的`ToolCall`对象，并将其添加到`tool_calls`列表中。在创建`ToolCall`对象时，会从当前工具调用中提取`id`、`name`和`arguments`属性。

5. 最后，函数返回填充了`ToolCall`对象的`tool_calls`列表。

**注意**：
- 使用此函数之前，确保`self.run.required_action`已经正确设置，否则将无法检索到工具调用信息。
- 由于这是一个异步函数，调用时需要使用`await`关键字。
- 函数返回的是一个`ToolCall`对象列表，需要确保调用此函数的上下文能够处理异步返回的列表结果。

**输出示例**：
假设当前运行中有两个工具调用，函数可能返回如下列表：
```python
[
    ToolCall(id="tool1", name="ToolName1", arguments={"arg1": "value1"}),
    ToolCall(id="tool2", name="ToolName2", arguments={"arg2": "value2"})
]
```
这个列表包含了两个`ToolCall`对象，每个对象都有其对应的`id`、`name`和`arguments`。
## AsyncFunctionDef submit_tool_outputs
**submit_tool_outputs函数**: 该函数的功能是提交工具输出。

该函数是一个异步函数，用于提交工具执行后的输出结果。它首先会调用`retrieve`方法来获取当前运行的状态。如果当前运行的状态不是`requires_action`，则会记录一条日志信息，并返回当前的运行对象。

如果运行状态是`requires_action`，则会通过客户端的`beta.threads.runs.submit_tool_outputs`方法，将工具的输出结果`tool_outputs`提交给服务端。提交后，它会更新当前的运行对象，并将其返回。

参数解析：
- `tool_outputs`: 一个包含字典的列表，每个字典代表一个工具的输出结果。

函数流程：
1. 调用`retrieve`方法获取当前运行的状态。
2. 检查当前运行状态是否为`requires_action`。
   - 如果不是，记录警告日志并返回当前运行对象。
   - 如果是，继续执行下一步。
3. 调用客户端的`submit_tool_outputs`方法，提交工具输出。
4. 更新并返回当前的运行对象。

**注意**：
- 在调用此函数之前，确保工具的输出已经准备好，并且当前运行状态为`requires_action`。
- 由于这是一个异步函数，调用时需要使用`await`关键字。
- 提交的工具输出应该是一个列表，列表中的每个元素是一个字典，代表一个工具的输出。

**输出示例**：
```python
{
    "id": "run_id_example",
    "status": "completed",
    "outputs": [...],  # 这里是工具输出的详细信息
    ...
}
```
在这个示例中，`submit_tool_outputs`函数返回了一个更新后的运行对象，其中包含了运行的ID、状态以及工具的输出信息等。
## AsyncFunctionDef retrieve
**retrieve函数**: 该函数的功能是异步检索并更新线程运行状态和步骤信息。

retrieve函数是一个异步方法，它的主要作用是从客户端的线程运行服务中检索当前线程的运行状态，并更新与该运行相关的步骤信息。该函数首先通过客户端接口获取当前运行的状态，并将其存储在self.run中。然后，它会异步地获取所有步骤信息，并更新内部状态，包括最后检索到的步骤和步骤消息。

在检索步骤信息时，函数会检查每个步骤的类型和状态。如果步骤类型为"message_creation"且状态为"completed"，则会进一步检索相关的消息内容，并对消息进行转换处理。

函数的最后，返回当前运行的状态，这可以用于判断线程是否已完成、过期、失败或被取消。

在项目中，retrieve函数被调用于以下场景：
1. 在`step`方法中，它被用于初始化运行状态，并在运行状态发生变化时，通过yield关键字返回新的状态。
2. 在`submit_tool_outputs`方法中，它被用于在提交工具输出之前检查当前运行的状态是否为"requires_action"。

**注意**:
- 由于retrieve函数是异步的，调用它时需要使用`await`关键字。
- 在使用该函数时，需要确保已经正确初始化了客户端，并且已经设置了必要的线程和运行ID。
- 函数内部使用了多个异步循环和异步API调用，因此在调用此函数的环境中也需要支持异步操作。

**输出示例**:
假设当前线程运行状态为"completed"，那么调用retrieve函数可能会返回如下值：
```python
"completed"
```
## AsyncFunctionDef cancel
**cancel函数**: 此函数的功能是取消当前正在运行的线程。

cancel函数是一个异步方法，它的主要作用是调用客户端的API来取消一个特定的运行中的线程。在XAgent的上下文中，线程通常指的是一系列的任务或者操作的集合，它们在后台异步执行。

在这个函数中，`self.client.beta.threads.runs.cancel`是一个API调用，它接受两个参数：`run_id`和`thread_id`。这两个参数分别代表了要取消的运行的ID和线程的ID。这些ID是在创建线程或运行时由系统生成的唯一标识符。

`self.run`是一个对象，它包含了关于当前运行状态的信息。通过调用`self.client.beta.threads.runs.cancel`方法并传递相应的ID，我们可以请求取消该运行。如果取消成功，API将返回更新后的运行状态，这个状态会被赋值给`self.run`。

这个函数是异步的，这意味着它在执行时不会阻塞其他代码的执行。在使用这个函数时，你需要在调用它的上下文中使用`await`关键字，以确保异步调用的正确处理。

**注意**:
- 在使用cancel函数时，确保你已经有了有效的`run_id`和`thread_id`，否则API调用可能会失败。
- 由于这是一个异步函数，确保在调用它的环境中正确处理异步操作。
- 在调用此函数后，应检查返回的运行状态，以确认线程是否已成功取消。

**输出示例**:
```python
{
    "id": "run_1234567890abcdef",
    "status": "cancelled",
    "thread_id": "thread_abcdef1234567890",
    ...
}
```
在这个示例中，返回的对象表示运行已被成功取消，其中包含了运行的ID、状态（已取消）以及线程的ID等信息。
***
