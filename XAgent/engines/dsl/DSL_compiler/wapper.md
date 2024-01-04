# ClassDef DSLRunningRecorder
**DSLRunningRecorder功能**: 该类的功能是记录DSL（Domain Specific Language，领域特定语言）运行时的结果。

DSLRunningRecorder类是一个用于记录DSL运行过程中产生的结果的记录器。它提供了一个结构来存储和管理运行时的数据，这些数据可能包括动作执行结果、工作流程执行结果以及AI函数执行结果。

类定义中包含了三个主要的属性，它们分别用于存储不同类型的运行结果：
- `action_result`: 一个字典，用于存储动作执行的结果。键是字符串类型，代表动作的名称或标识符；值是`TestResult`类型，代表该动作的测试结果。
- `workflow_result`: 一个字典，用于存储工作流程执行的结果。其结构与`action_result`相同。
- `ai_function_result`: 一个字典，用于存储AI函数执行的结果。其结构也与`action_result`相同。

类中定义了两个方法：
1. `__init__`: 构造函数，在创建DSLRunningRecorder实例时被调用。它会调用`fresh`方法来初始化记录器的状态。
2. `fresh`: 该方法用于刷新或重置记录器的内容，将所有结果字典清空，以便记录器可以重新开始记录新的运行结果。

`get_runtime_result`方法目前是一个空方法，根据方法名和上下文推测，该方法的目的可能是用于在DSL运行结束后，提取和返回运行时的结果。由于方法体为空，具体的实现细节和返回值类型未知。

**注意**：
- `TestResult`类型没有在代码片段中定义，因此需要在其他部分的代码或文档中查找其定义，以了解它的结构和用途。
- `get_runtime_result`方法尚未实现，如果需要使用该方法，开发者需要根据实际需求完成相应的代码逻辑。
- 在使用DSLRunningRecorder类时，需要注意它是如何与DSL的执行流程集成的，以及如何正确地将执行结果存储到对应的字典中。
## FunctionDef __init__
**__init__ 函数**: 该函数的功能是初始化对象并设置其初始状态。

详细代码分析和描述如下：

`__init__` 函数是一个特殊的方法，它在Python中被称为构造函数。当一个新的对象实例被创建时，构造函数会被自动调用。在这个函数中，可以设置对象在创建时需要具备的任何初始状态或属性。

在这段代码中，`__init__` 函数没有接受任何参数（除了自身的引用`self`），它的作用是调用了`self.fresh()`方法。这表明在创建对象的实例时，会立即调用`fresh`方法来重置或初始化对象的某些状态。

由于代码中没有提供`fresh`方法的具体实现，我们无法确切知道它会执行哪些操作。通常，这样的方法可能会用于清除对象的属性，或者将它们重置为默认值。

**注意**：在使用这段代码时，开发者需要确保`fresh`方法已经在类中被定义，并且可以在不需要额外参数的情况下被调用。如果`fresh`方法依赖于外部状态或需要参数，那么在调用`__init__`时可能会出现错误。此外，如果`fresh`方法中包含重要的初始化逻辑，那么在创建对象的实例后，这些逻辑将会被立即执行。因此，开发者需要了解`fresh`方法的内部工作机制，以确保对象的正确初始化。
## FunctionDef fresh
**fresh函数**: 该函数的功能是刷新类实例中的结果存储字典。

该`fresh`函数定义在`wapper.py`文件中，它是一个成员方法，用于初始化或重置类实例中用于存储结果的字典。具体来说，该函数会重置三个字典：`action_result`、`workflow_result`和`ai_function_result`。这三个字典都是以字符串为键，`TestResult`类型为值的字典，用于存储不同类型的测试结果。

在`fresh`函数中，每个字典都被初始化为空字典，这意味着之前存储的所有结果都将被清除。这样做的目的可能是为了在开始新的测试或操作前，确保没有旧的测试结果影响到即将进行的测试。

在类的构造函数`__init__`中调用了`fresh`方法，这表明每当创建类的新实例时，都会自动刷新这些结果存储字典，确保每个新实例都是从干净的状态开始。

**注意**：
- 使用`fresh`函数时，需要确保`TestResult`类型已经被正确定义，并且可以被该类访问。
- 在调用`fresh`函数后，之前存储在`action_result`、`workflow_result`和`ai_function_result`字典中的所有数据都将丢失，因此在调用该函数前应确保不再需要这些数据。
- 由于`fresh`函数在类的构造函数中被调用，因此在创建类的实例时无需手动调用`fresh`，除非在实例的生命周期中需要手动重置这些字典。
## FunctionDef get_runtime_result
**get_runtime_result 函数**: 该函数的功能是取回DSL运行结果。

该函数`get_runtime_result`目前是一个空函数，即它的函数体中没有任何执行代码。通常，这种情况意味着该函数是一个待实现的功能，或者是一个接口的一部分，需要在子类或者后续的代码中被覆盖（override）或具体实现。

在当前的上下文中，函数名`get_runtime_result`暗示它应该在DSL（领域特定语言）代码执行后，返回运行结果。然而，由于函数体中只有一个`pass`语句，我们无法得知它将如何处理或返回结果。在实际使用中，开发者需要根据DSL运行环境和需求，实现该函数的具体逻辑，以便能够正确地获取并返回DSL代码的运行结果。

**注意**:
- 由于`get_runtime_result`函数目前没有实现任何功能，直接调用它将不会有任何效果。
- 开发者在使用该函数之前，需要确保在相应的类或模块中实现了该函数的具体逻辑。
- 如果该函数是设计为被子类继承并重写的，那么在子类中必须提供具体的实现细节，否则调用时仍然不会有任何实际的运行结果。
- 在文档或代码注释中应该明确指出该函数的预期行为和使用场景，以便其他开发者理解和正确使用该函数。
***
# FunctionDef dsl_action_wrapper
**dsl_action_wrapper函数**: 该函数的功能是对DSL动作函数进行包装，以便在执行前后进行记录和日志输出。

dsl_action_wrapper函数是一个装饰器，它接受一个异步函数`func`作为参数，并返回一个新的异步函数`wrapper`。这个新函数在调用原函数之前和之后，会执行一些额外的操作，主要用于测试和记录DSL动作的执行情况。

详细代码分析如下：

1. `wrapper`函数是一个异步函数，它可以接受任意数量的位置参数`*args`和关键字参数`**kwargs`。
2. 在`wrapper`函数内部，首先检查`running_recoder.action_result`字典中是否已经有了以`func.__name__`（即原函数名）为键的记录。如果没有，就在字典中为这个键创建一个空列表，用于存储该函数的运行结果。
3. 创建一个`TestResult`对象`temp_running_result`，它记录了函数的输入参数`args`和`kwargs`。这里使用`deepcopy`来确保参数的副本不会被后续操作修改。
4. 将`temp_running_result`对象添加到`running_recoder.action_result[func.__name__]`列表中，这样就记录了当前函数的一次调用及其输入参数。
5. 使用`recorder.log`输出一条日志信息，内容包括当前执行的动作函数名称和这是第几次调用。
6. 调用原始的动作函数`func`，并将结果存储在变量`result`中。
7. 函数最后返回`result`，即原函数的执行结果。

**注意**：
- 使用此装饰器时，需要确保`running_recoder`和`recorder`是有效的，并且它们具有`action_result`属性和`log`方法。
- 被装饰的函数`func`应该是一个异步函数，因为`wrapper`函数使用了`await`关键字。
- 由于使用了`deepcopy`，需要注意原函数的输入参数不应包含无法被深拷贝的对象。

**输出示例**：
由于`dsl_action_wrapper`是一个装饰器，它本身不直接产生输出。它的输出取决于被装饰的函数`func`的返回值。例如，如果`func`返回一个字符串"Hello, World!"，那么`wrapper`函数也会返回"Hello, World!"。同时，在`running_recoder.action_result`中会记录下相关的调用信息和参数。
***
# FunctionDef dsl_code_wrapper
**dsl_code_wrapper函数**: 该函数的作用是拦截并记录DSL代码运行时所有函数的执行情况。

dsl_code_wrapper函数是一个装饰器，它接收一个异步函数func作为参数，并返回一个新的异步函数wrapper。这个装饰器的主要目的是在DSL（Domain Specific Language）代码执行时，拦截并记录每个函数的调用情况，包括输入参数和调用次数。

当被装饰的函数func被调用时，wrapper函数将执行以下步骤：

1. 首先检查全局变量running_recoder中的workflow_result字典是否已经记录了func函数的执行结果。如果没有，将func函数名作为键，初始化一个空列表作为值，用于存储后续的执行结果。

2. 创建一个TestResult对象，将当前调用的输入参数（args和kwargs）的深拷贝作为TestResult对象的属性。这样做是为了记录函数调用时的初始状态，避免后续的操作影响记录的准确性。

3. 将创建的TestResult对象追加到running_recoder.workflow_result字典中func函数名对应的列表中。这样可以记录每次调用func函数时的输入参数。

4. 使用recorder.log函数输出日志信息，包括当前执行的函数名func.__name__和这是第几次调用该函数。

5. 调用原始的func函数，并传入相应的参数args和kwargs，执行函数逻辑。

6. 将func函数的执行结果返回。

通过这个装饰器，可以在DSL代码的执行过程中，实时监控和记录每个函数的调用情况，这对于调试和分析DSL代码的执行流程非常有帮助。

**注意**：
- 由于dsl_code_wrapper是一个装饰器，它应该被应用于异步函数上。在使用时，需要确保func是一个异步函数，即使用async关键字定义的函数。
- 在使用此装饰器时，需要确保全局变量running_recoder已经被正确初始化，并且具有workflow_result属性。
- 此装饰器依赖于deepcopy函数来创建输入参数的深拷贝，因此在使用前需要确保已经从copy模块导入了deepcopy函数。
- recorder.log函数用于输出日志信息，需要确保recorder对象已经被正确初始化，并且log方法可以正常使用。

**输出示例**：
由于dsl_code_wrapper函数是一个装饰器，它本身不直接产生输出结果。它的输出结果取决于被装饰的func函数的返回值。例如，如果func函数的返回值是一个字符串"Hello, World!"，那么在调用被dsl_code_wrapper装饰后的func函数时，最终的返回值也将是"Hello, World!"。同时，调用信息将被记录在running_recoder的workflow_result中。
## AsyncFunctionDef wrapper
**wrapper函数**: 此函数的功能是拦截并记录DSL代码运行时所有函数的执行情况。

wrapper函数是一个异步函数，它的主要作用是在DSL代码执行过程中，对每个函数调用进行拦截和记录。它通过装饰器模式，将原始函数包装在一个新的异步函数中，以便在原始函数执行前后进行额外的操作。

详细代码分析如下：

1. wrapper函数接受任意数量的位置参数(*args)和关键字参数(**kwargs)。
2. 函数首先检查`running_recoder.workflow_result`字典中是否已经存在以当前函数名`func.__name__`为键的条目。如果不存在，则为该函数名创建一个空列表，用于存储运行结果。
3. 接下来，创建一个`TestResult`实例，将当前函数的输入参数和关键字参数的深拷贝作为实例的属性。这样做是为了记录函数的调用情况，包括它接收到的参数。
4. 将这个`TestResult`实例添加到`running_recoder.workflow_result[func.__name__]`列表中，表示该函数的一个新的运行实例。
5. 使用`recorder.log`函数记录当前函数的执行情况，包括函数名和这是第几次执行该函数。
6. 然后，使用await关键字调用原始函数`func`，并将所有接收到的参数传递给它。由于`func`可能是异步的，因此这里使用await确保函数执行完成。
7. 最后，返回原始函数的执行结果。

**注意**：
- wrapper函数是一个异步函数，因此在调用时需要使用`await`关键字。
- 由于wrapper函数设计为装饰器，因此在使用时需要将它应用到另一个函数上，如`@dsl_code_wrapper`。
- wrapper函数依赖于外部的`running_recoder`对象和`TestResult`类，这意味着在使用wrapper函数之前，需要确保这些依赖已经正确初始化和配置。

**输出示例**：
由于wrapper函数返回的是原始函数`func`的执行结果，因此输出示例将取决于`func`的具体实现。例如，如果`func`返回一个字符串"Hello, World!"，那么wrapper函数也将返回"Hello, World!"。
***
