# ClassDef DSLRunningException
**DSLRunningException函数**: 这个类的函数是用于封装报错类型的，可以重载自定义的错误类型。它包含以下方法：

- `__init__(self, message)`: 初始化函数，接受一个错误信息作为参数，并调用父类的初始化函数。同时，它还初始化了一个空的代码堆栈和错误信息。

- `add_context_stack(self, code_context)`: 将代码上下文添加到代码堆栈中。参数`code_context`是要添加到堆栈的代码上下文。

**注意**: 这个类的目的是用于封装报错类型，可以根据自己的需求重载错误类型。在使用时，可以通过调用`add_context_stack`方法将代码上下文添加到代码堆栈中，以便更好地定位错误。
## FunctionDef add_context_stack
**add_context_stack函数**：该函数的作用是将代码上下文添加到代码堆栈中。

该函数接受一个参数code_context，表示要添加到堆栈中的代码上下文。

该函数没有返回值。

在函数内部，它将code_context添加到self.code_stack中。

使用示例：
```python
code_context = "some code context"
add_context_stack(code_context)
```

**注意**：在使用该函数时，需要注意以下几点：
- code_context参数应为任意类型的数据。
- 该函数没有返回值，只是将code_context添加到堆栈中。
***
# ClassDef anonymous_class
**anonymous_class功能**：这个类的功能是执行与DSLAction节点相关联的动作。

这个类定义了一个匿名类，它包含了执行DSLAction节点所需的逻辑。它的构造函数接收一个DSLAction节点和一个动作执行器（action_executor），这个执行器是一个可调用对象，用于执行与节点相关的动作。

类中的`run`方法接收输入参数和可选的输入数据，调用动作执行器，并处理执行过程中可能出现的错误。如果执行器返回错误信息，`run`方法将抛出一个DSLRunningException异常。

在`run`方法中，首先调用动作执行器，传入动作节点、输入参数和输入数据。动作执行器返回输出数据和错误信息。如果有错误信息，方法将创建一个DSLRunningException异常并抛出；如果没有错误，方法将返回输出数据。

在项目中，这个类被用在`DSLActionRunner`类中。`DSLActionRunner`类的`__call__`方法中，通过创建一个`anonymous_class`的实例，并将其作为局部函数`transparent_function`添加到命名空间中，以便在执行DSL节点代码时可以调用。这样，当DSL节点代码执行时，它会调用`transparent_function`，实际上就是调用`anonymous_class`的`run`方法来执行动作。

**注意**：
- 在使用这个类时，需要确保传递给构造函数的`action_executor`是一个正确的可调用对象，它应该能够接收动作节点、输入参数和输入数据，并返回输出数据和错误信息。
- 在处理异常时，应该注意捕获DSLRunningException异常，并根据需要处理异常情况。

**输出示例**：
假设动作执行器成功执行了动作并返回了一些输出数据，没有错误信息，那么`run`方法的返回值可能如下：
```python
{
    'result': '动作执行的结果数据',
    'status': 'success'
}
```
如果动作执行器在执行过程中遇到了错误，`run`方法将抛出一个DSLRunningException异常，异常信息可能如下：
```python
DSLRunningException: "执行动作时发生错误，错误信息"
```
## FunctionDef run
**run函数**: 此函数的功能是执行给定输入数据和参数的函数。

run函数是一个核心执行器，负责根据提供的输入数据和参数运行特定的功能。它是DSL编译器的一部分，通常用于执行由DSL（领域特定语言）定义的任务。

详细代码分析和描述：
- `run`函数接受两个参数：`input_params`和`input_data`。
- `input_params`是一个字典（dict），包含了运行函数所需的参数。
- `input_data`是任意类型，代表函数的输入数据，可以是字符串、数字、列表、字典等。
- 函数内部调用`action_executor`方法，传入当前节点（`self.node`）以及`input_params`和`input_data`。
- `action_executor`方法执行具体的动作，并返回两个值：`output_data`和`error`。
- 如果`error`不为空字符串，说明在执行过程中遇到了错误，此时会抛出一个`DSLRunningException`异常。
- 如果没有错误发生，函数则返回`output_data`，即动作执行后的输出数据。

**注意**：
- 在使用`run`函数时，需要确保传入的参数和输入数据符合预期的格式和类型，否则可能会导致执行失败。
- 当函数执行出错并抛出`DSLRunningException`异常时，需要有相应的异常处理机制来捕获和处理这些异常。

**输出示例**：
假设`action_executor`方法成功执行并返回了一个字符串"Hello, World!"作为`output_data`，且没有错误发生（`error`为空字符串），那么`run`函数的返回值将会是：
```python
"Hello, World!"
```
如果`action_executor`方法执行过程中发生错误，例如错误信息为"Invalid input"，那么`run`函数将会抛出一个`DSLRunningException`异常：
```python
raise DSLRunningException("Invalid input")
```
***
# ClassDef DSLActionRunner
**DSLActionRunner 类的功能**：这个类的功能是执行DSL（Domain Specific Language）中定义的动作节点，并返回动作执行的结果。

DSLActionRunner 类是一个可调用对象，它封装了对DSL动作节点的执行逻辑。它的构造函数接收一个DSL动作节点（DSLAction），全局变量字典，命名空间字典，以及一个可调用的动作执行器。这个类的主要职责是在被调用时执行与DSL动作节点相关的代码，并处理输入输出数据。

**详细代码分析**：
1. `__init__` 方法初始化了DSLActionRunner实例，存储了动作节点、全局变量、命名空间和动作执行器。
2. `__call__` 方法是DSLActionRunner的核心，它被设计为一个可调用方法，允许将DSLActionRunner的实例像函数一样被调用。
3. 在 `__call__` 方法中，首先更新了节点的运行时信息，记录了访问次数和输入数据。
4. 定义了输入数据和输出数据的变量名，并将它们添加到命名空间中。
5. 使用 `exec` 函数执行节点代码，这段代码是通过DSL节点的 `to_code` 方法转换得到的Python代码。
6. 如果执行成功，它将从局部变量中提取输出数据，并更新节点的运行时信息，然后返回输出数据。
7. 如果执行过程中出现异常，它将捕获异常并构造一个DSLRunningException异常对象，添加上下文信息，并最终抛出这个异常。

**注意事项**：
- 在使用DSLActionRunner时，需要确保传入的DSLAction节点是正确配置的，并且提供的全局变量和命名空间是有效的。
- 动作执行器应该是一个可调用对象，它将被用于执行动作节点中定义的具体操作。
- 由于使用了 `exec` 函数执行动态生成的代码，需要注意代码安全性，避免执行不安全的代码。
- 异常处理部分使用了断言和异常上下文堆栈，这有助于调试和定位问题，但也意味着调用者需要准备处理可能抛出的DSLRunningException异常。

**输出示例**：
由于DSLActionRunner的输出取决于执行的DSL动作节点代码，因此输出示例将因具体情况而异。但一般来说，如果动作执行成功，它将返回动作节点代码执行后的输出数据；如果执行失败，它将抛出一个包含错误信息和上下文的DSLRunningException异常。
***
# ClassDef DSLRunner
**DSLRunner函数**：这个类的函数是用来执行DSL工作流的。

该类的构造函数接受以下参数：
- dsl: DSL对象，表示要执行的DSL工作流。
- workflow_code: 字符串，表示工作流的代码。
- code_name: 字符串，表示代码的名称。
- global_vars: 字典，表示全局变量。
- name_space: 字典，表示命名空间。

该类有一个`__call__`方法，用于执行工作流。它接受一个可选的input_data参数，表示要传递给工作流的输入数据。该方法的返回值是工作流生成的输出数据。

在执行工作流之前，该方法会更新DSL的运行时信息，并将输入数据存储在DSL的last_runtime_info对象中。

然后，该方法会将全局变量和DSLRouteResult对象添加到命名空间中，并根据代码名称生成输出数据的变量名。

接下来，根据代码名称的不同，将不同的代码片段添加到工作流代码中。

如果代码名称是"mainWorkflow"，则将输入数据添加到命名空间中，并将其作为参数传递给工作流函数。

如果代码名称不是"mainWorkflow"，则直接调用工作流函数。

在执行工作流代码之前，该方法会捕获可能发生的异常，并将其转换为DSLRunningException异常。

如果发生异常，该方法会获取异常的堆栈信息，并将其添加到DSLRunningException对象的code_stack属性中。

最后，该方法会抛出DSLRunningException异常。

**注意**：在使用DSLRunner类时，需要注意以下几点：
- 构造函数的参数需要按照指定的类型进行传递。
- 执行工作流时，可以传递输入数据作为参数。
- 如果发生异常，可以通过捕获DSLRunningException异常来获取异常信息。

**输出示例**：假设工作流的输出数据为{"result": "success"}，则执行工作流的返回值为{"result": "success"}。
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化一个DSL执行器的实例。

详细代码分析和描述如下：

- `self.code_name`：这是一个字符串属性，用于存储代码的名称。它在初始化时从参数`code_name`中获取其值，这个名称可能用于日志记录或其他需要标识代码的场景。
- `self.name_space`：这是一个字典属性，用于存储命名空间中的变量。它从参数`name_space`中获取其值，这个命名空间可能包含了执行DSL代码时需要的变量和函数。
- `self.dsl`：这是一个`DSL`类型的属性，它持有传入的DSL对象。这个对象可能包含了解析DSL代码所需的方法和属性。
- `self.workflow_code`：这个属性存储了工作流代码，它从参数`workflow_code`中获取其值。这段代码可能是DSL代码的一部分，用于定义工作流的逻辑。
- `self.global_vars`：这是一个属性，用于存储全局变量。它从参数`global_vars`中获取其值，这些全局变量可能在DSL代码执行过程中被引用或修改。

**注意**：
- 在使用这个`__init__`函数初始化DSL执行器实例时，需要确保传入的参数类型和预期一致。例如，`dsl`参数应该是一个有效的`DSL`对象，`name_space`应该是一个字典。
- `global_vars`参数通常用于存储在DSL代码执行过程中需要持续访问或修改的变量，因此在初始化时应该提供一个合适的字典。
- 这个初始化函数不返回任何值，它仅用于设置实例的内部状态，以便后续方法可以正确地使用这些状态。
## FunctionDef __call__
**__call__函数**: 该函数的功能是执行给定输入数据的工作流。

__call__函数是一个特殊的方法，它允许对象的实例像函数那样被调用。在这段代码中，__call__函数用于执行一个工作流，并处理输入和输出数据。

函数首先将输入数据`input_data`作为工作流的输入。它记录了当前工作流被调用的次数，并将输入数据保存在`dsl.last_runtime_info`对象中。

接着，函数设置了命名空间`name_space`中的全局变量，并定义了输出数据的变量名`output_data_var_name`。如果工作流的名称是`mainWorkflow`，则会特别处理输入数据，将其放入命名空间，并构造工作流代码以调用主工作流函数。

函数使用`exec`执行工作流代码。如果执行成功，它会从局部变量中获取输出数据，并将其深度复制到`dsl.last_runtime_info.output_data`中，然后返回输出数据。

如果在执行过程中遇到异常，函数会捕获异常并构造一个`DSLRunningException`异常对象。它会从异常的回溯中获取错误代码的位置，并将相关的错误代码上下文添加到异常对象中，以便于调试。

最后，如果发生了异常，函数会抛出`DSLRunningException`异常。

**注意**:
- 在使用此函数时，需要确保`input_data`格式正确，并且能够被工作流正确处理。
- 如果在执行工作流代码时发生错误，会抛出`DSLRunningException`异常，调用者需要捕获并处理这个异常。
- 由于使用了`exec`执行代码，需要注意代码安全性，避免执行不安全的代码。

**输出示例**:
假设工作流执行成功，没有发生异常，函数可能返回如下格式的数据：
```python
{
    'result': '工作流执行结果',
    'data': '处理后的数据'
}
```
如果执行失败，函数将抛出`DSLRunningException`异常，异常对象中包含错误信息和上下文。
***
# AsyncFunctionDef run_DSL
**run_DSL函数**: 此函数的功能是执行DSL（领域特定语言）代码，并根据提供的起始命名空间、动作执行器、输入数据和管道参数来初始化和修改所有访问节点的信息。

详细代码分析和描述如下：

1. 函数接收四个参数：`dsl` 是 DSL 类的实例，包含了要执行的 DSL 代码和相关信息；`start_name_space` 是一个字典，表示执行代码前的起始命名空间；`action_executor` 是一个执行动作的函数或对象；`input_data` 和 `pipeline_params` 是可选参数，分别用于提供输入数据和管道参数。

2. 函数首先调用 `compile_workflow` 函数来编译 DSL 代码，得到全部代码、全局变量代码、函数代码和工作流代码。

3. 然后，函数使用 `highlight_code` 函数来高亮显示编译后的全部代码，并打印输出。

4. 接下来，函数初始化 `dsl` 对象的 `last_runtime_info` 属性，以及 `dsl` 中每个动作节点的 `last_runtime_info` 属性。这些属性用于记录测试结果和运行状态。

5. 函数创建一个新的命名空间 `name_space`，并根据 `dsl` 中的动作列表来填充这个命名空间。对于类型为 `Switch` 的动作节点，会创建一个 `DSLRunner` 实例；对于其他类型的动作节点，会创建一个 `DSLActionRunner` 实例。

6. 在命名空间中，还会创建一个名为 `mainWorkflow` 的 `DSLRunner` 实例，它代表整个工作流的执行入口。

7. 接着，函数为命名空间中的每个实例设置 `name_space` 属性，确保它们可以相互访问。

8. 函数尝试异步执行 `mainWorkflow`，并传入 `input_data` 作为输入。如果在执行过程中抛出 `DSLRunningException` 异常，则会捕获该异常，并构建一个错误堆栈字符串 `error_stack_str`，最后打印输出这个错误堆栈。

**注意**：
- 在使用 `run_DSL` 函数时，需要确保传入的 `dsl` 参数是正确配置的 DSL 实例，且包含了有效的 DSL 代码和动作列表。
- `start_name_space` 应该是一个字典，包含了执行 DSL 代码前所需的所有起始变量和函数。
- `action_executor` 参数需要是一个能够执行 DSL 中定义的动作的函数或对象。
- `input_data` 和 `pipeline_params` 参数是可选的，它们分别用于提供给工作流的输入数据和管道参数。
- 当执行 DSL 代码时，如果遇到错误或异常，错误信息会通过打印输出的方式提供给开发者，因此开发者需要注意检查控制台的输出，以便及时发现并解决问题。
***
