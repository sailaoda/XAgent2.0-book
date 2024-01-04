# ClassDef ActionsReflection
**ActionsReflection 功能**: 该类的功能是选择有用的行动并为未来的行动提供建议。

ActionsReflection 类是一个数据模型，它继承自 BaseModel。这个类的主要作用是在执行任务时，筛选出关键的成功行动，并基于这些行动的结果提供对未来行动的建议。这个类包含两个字段：`key_actions` 和 `suggestions`。

- `key_actions`: 这是一个整数列表，用于记录最多五个关键的成功行动。这些行动的输出包含对当前任务目标或描述至关重要的内容，例如搜索结果、有用的网页内容、文件内容等。这个字段的目的是帮助识别在任务执行过程中哪些行动是有效的，并且其结果对于实现任务目标具有重要意义。

- `suggestions`: 这是一个字符串，用于提供最多三条建议。这些建议是基于之前的成功和失败行动学习得来的，目的是为未来的行动提供指导。这可以帮助改进任务执行策略，提高效率和成功率。

在项目中，`ActionsReflection` 类被调用于 `XAgent/ai_functions/functions/context.py` 文件中的 `actions_reflection` 异步函数。该函数的作用是根据当前任务的阶段目标，选择解决或满足该目标的关键行动，并在此基础上为未来的任务解决方案提供建议。函数通过接收当前任务描述和已执行的行动作为输入，并要求输出标准的 JSON 格式的 `ActionsReflection` 对象。

**注意**:
- 在使用 `ActionsReflection` 类时，需要确保 `key_actions` 字段中的行动数量不超过五个，并且每个行动都是对当前任务有实质性帮助的。
- `suggestions` 字段应包含最多三条建议，这些建议应当是实用的，并且能够基于之前的行动结果来指导未来的行动。
- 在实际应用中，需要根据任务的具体情况和行动的结果来动态生成这些字段的内容。
- 由于 `ActionsReflection` 类是作为数据模型使用的，因此在实现时应注意数据的准确性和相关性，以确保模型的有效性。
***
# AsyncFunctionDef actions_reflection
**actions_reflection函数**: 该函数的功能是选择关键动作来解决或满足当前任务的阶段目标，并提供关键动作的输出以解决后续任务。此外，还应提出未来动作的建议。

该`actions_reflection`函数是一个异步函数，它接收两个关键参数：`current_task`和`actions`。这两个参数分别代表当前的任务描述和已经执行的动作描述。函数的目的是基于当前任务的阶段目标，从已执行的动作中筛选出关键动作，并对未来可能执行的动作给出建议。

函数的详细分析如下：

- `current_task`参数：这是一个字符串参数，代表当前正在处理的任务的描述。它应该包含当前任务的详细信息，以便函数能够理解任务的上下文并据此选择关键动作。

- `actions`参数：这也是一个字符串参数，代表到目前为止已经执行的动作的描述。这些动作的描述应该详细到足以让函数分析哪些动作是解决当前任务的关键。

- 函数返回值：函数的返回类型是`ActionsReflection`。这意味着函数的输出应该是一个标准的JSON格式，其中包含关键动作的信息以及对未来动作的建议。

函数内部的文档字符串（docstring）提供了一个模板，说明了如何格式化`current_task`和`actions`参数的输入内容，并强调了输出应该是标准的JSON格式。

**注意**：

- 由于这是一个异步函数，调用它时需要使用`await`关键字，或者在其他异步函数中调用它。

- 函数的返回类型`ActionsReflection`没有在代码片段中定义，因此需要在相应的模块中查找或定义这个类型，以确保函数能够返回正确的数据结构。

- 输入的`current_task`和`actions`应该是详细且格式正确的字符串，以便函数能够正确解析并提取有用信息。

- 输出的JSON格式需要符合特定的结构和规范，以便其他系统组件可以正确地解析和使用这些数据。

- 在实际部署和使用这个函数之前，需要确保所有相关的环境和依赖都已经正确设置和配置。
***
# ClassDef ActionsSelection
**ActionsSelection功能**: 该类的功能是理解行动的意图并选择有用的行动。

ActionsSelection类是一个数据模型，它用于理解一系列行动的意图，并从中挑选出关键的行动。这个类继承自BaseModel，通常用于数据验证和数据传输对象（DTO）的创建。

该类包含两个字段：
1. `intentions`：这是一个字符串字段，用于存储对行动意图的简短总结。这个字段的描述指出，意图应该是50到100个单词的简短描述。
2. `critical_actions`：这是一个整数列表，用于存储关键行动的索引。这个字段的描述指出，列表的最大长度是5，即最多只能选择5个关键行动。

在项目中，ActionsSelection类被用于`understand_actions_with_intentions`异步函数中，该函数的目的是基于长期信息、自我检查和行动日志来理解行动的意图，并从中选出最关键的行动。函数的参数包括长期上下文（longterm_context）、自我检查（self_examination）和行动（actions）。函数的输出是一个ActionsSelection对象，其中包含了行动的意图总结和关键行动的索引列表。

**注意**：
- 在使用ActionsSelection类时，需要确保`intentions`字段的内容是简洁且具体的，能够反映行动的真实意图，并且长度在50到100个单词之间。
- `critical_actions`字段应该只包含最多5个元素，这些元素是行动索引的列表，代表了被认为是最关键的行动。
- 在调用`understand_actions_with_intentions`函数时，需要提供完整的上下文信息，以便正确理解行动的意图并选择关键行动。
- 应避免提供与日志无关的信息，并保持观察到的关键信息，这些信息在未来可能会有用。
***
# AsyncFunctionDef understand_actions_with_intentions
**understand_actions_with_intentions 函数**: 该函数的功能是理解并筛选出最关键的行动，并总结其意图。

该函数`understand_actions_with_intentions`是一个异步函数，它接收三个关键的字符串参数：长期上下文(`longterm_context`)、自我检查(`self_examination`)和行动(`actions`)。函数的目的是让调用者反思自己的行动和意图，然后从中选择最关键的行动，并以第一人称的方式简洁、具体地进行总结。

函数的输入参数包括：
- `longterm_context`：长期上下文信息，提供给函数以便理解行动发生的背景。
- `self_examination`：自我检查的记录，即最近行动的日志，用于反思。
- `actions`：实际执行的行动列表。

函数的返回值是`ActionsSelection`对象，这个对象包含了关键行动的选择和意图的总结。

在项目中，`understand_actions_with_intentions`函数被`XAgent/agent/summarize.py`文件中的`summarize_tool_calls`函数调用。在`summarize_tool_calls`函数中，它用于对工具调用(tool calls)进行总结，返回工具调用的描述、额外的想法以及令牌数。函数首先计算输入的令牌数，然后将工具调用转换为字符串描述，并调用`understand_actions_with_intentions`函数来获取关键行动和意图。最后，它将这些信息整合，返回工具调用的描述、意图和输入令牌数。

**注意**：
- 在使用`understand_actions_with_intentions`函数时，需要确保提供的日志信息是完整的，因为函数不会提供日志中未显示的信息。
- 在总结观察到的关键信息时，应保持对未来可能有用的信息的关注。
- 由于这是一个异步函数，调用时需要使用`await`关键字，并且调用者应该在支持异步操作的环境中运行。
- 函数的返回值`ActionsSelection`需要有明确的结构定义，以便调用者可以正确地处理返回的数据。
***
# ClassDef WorkingContextSummary
**WorkingContextSummary 类的功能**: 此类的功能是解析并总结第一人称的工作上下文。

WorkingContextSummary 类继承自 BaseModel，用于创建一个工作上下文的简要概述对象。该类包含两个字段：`summary` 和 `self_examination`。

- `summary` 字段是一个字符串，用于存储工作上下文的简短总结，其描述为“50 - 100字，工作上下文的简短总结”。这个字段要求用户提供一个简洁的描述，介于50到100个词之间，概括当前工作环境的情况。

- `self_examination` 字段也是一个字符串，用于存储自我检查的内容。如果用户有任何自我检查的内容，可以写在这里，否则可以保留为空。该字段的描述为“如果你有任何自我检查，请在这里写下。否则请保留为空。确保内容具有指导意义和意义。”此字段使用了别名 `self-examination`，并且默认工厂函数设置为返回 `None`，这意味着如果不提供值，该字段将默认为空。

在项目中，`WorkingContextSummary` 类被用于 `XAgent/ai_functions/functions/context.py` 文件中的 `summarize_working_context` 异步函数。此函数接收长期上下文信息和当前工作上下文的日志，要求以第一人称简洁、具体地总结最近的思考和行动。

函数的注释中提到了一些使用时需要注意的点：
- 不要提供日志中未显示的信息。
- 保持观察中的关键信息，这些信息将来会有用。

**注意**：
- 在使用 `WorkingContextSummary` 类时，需要确保 `summary` 字段的内容在50到100个词之间，并且能够准确概括工作上下文。
- `self_examination` 字段是可选的，如果用户没有自我检查的内容，可以不填写。如果填写，需要确保内容是有指导意义和意义的。
- 在调用 `summarize_working_context` 函数时，应当遵循其注释中的指导原则，确保提供的信息准确且有助于未来的工作。
***
# AsyncFunctionDef summarize_working_context
**summarize_working_context 函数**: 该函数的功能是根据提供的长期信息和工作上下文，生成一个关于最近思考和行动的简明且具体的第一人称总结。

该`summarize_working_context`函数是一个异步函数，它接收两个关键字参数：`longterm_context`和`working_context`。这两个参数分别代表长期信息和当前的工作上下文。函数的目的是将这些信息综合起来，提炼出一个简洁且具体的总结。这个总结将以第一人称的形式表达，便于理解和记录。

在`longterm_context`参数中，应该包含长期的、重要的信息，这些信息可能对未来的决策或行动有帮助。而`working_context`参数则包含了最近的思考和行动日志。函数的注释中强调了两点注意事项：一是不要提供日志之外的信息，二是在观察中保持对未来可能有用的关键信息。

在项目中，`summarize_working_context`函数被调用于`XAgent/agent/base.py`文件中。在这里，该函数用于创建一个上下文总结任务（`_context_summary_task`），该任务将异步执行，总结最新的工作上下文。这个调用发生在一个名为`_setup_summary_tasks`的方法内，该方法检查配置中是否启用了工作上下文的总结，并在条件满足时创建相应的异步任务。

**注意**：
- 由于`summarize_working_context`是一个异步函数，调用它时需要使用`asyncio.create_task`来创建一个异步任务，或者在另一个异步函数中使用`await`关键字来等待它的执行结果。
- 在使用此函数时，需要确保传入的`longterm_context`和`working_context`参数是字符串类型，并且包含了正确和相关的信息，以便函数能够生成有效的总结。
- 函数的返回类型为`WorkingContextSummary`，这意味着需要有一个相应的数据结构来接收总结结果。在文档中没有提供这个类型的具体定义，因此在实际使用时，需要查看项目中的其他部分以了解这个类型的详细信息。
- 函数注释中的大括号`{}`用于格式化字符串，这在Python中通常用于字符串插值，但在这里它们可能只是为了在文档中标识参数的位置。在实际的函数实现中，需要替换为相应的变量值。
***
