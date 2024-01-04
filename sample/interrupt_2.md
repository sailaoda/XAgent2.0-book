# AsyncFunctionDef main
**main函数**: 该函数的功能是演示如何使用异步编程来运行计划引擎和响应引擎，并在中途通过用户输入来中断执行。

该代码段定义了一个名为`main`的异步函数，用于演示如何在XAgent框架中创建和运行计划节点（PlanNode）以及如何处理中断（INTERRUPT）。

首先，函数从`XAgent.models`导入`PlanNode`类，从`XAgent.engines`导入`PlanEngine`和`ReActEngine`类，以及从`XAgent.interrput`导入`set_interrupt`函数和`INTERRUPT`全局变量。

接着，定义了一个名为`DummyEngine`的类，它继承自`BaseEngine`。`DummyEngine`类重写了`run`方法，该方法首先调用`lazy_init`进行初始化，然后使用`asyncio.sleep`模拟长时间的异步操作。在等待20秒后，通过调用`set_interrupt`函数设置中断标志。再次等待30秒后，程序会提示用户输入中断消息，并将该消息设置到全局的`INTERRUPT`变量中。

在`main`函数中，创建了一个`PlanEngine`实例和一个`ReActEngine`实例，并传入了配置对象`CONFIG`。然后，创建了一个`PlanNode`实例，代表一个计划节点，其目标是"Find primes below 100"。

随后，创建了一个`DummyEngine`实例，并使用`asyncio.create_task`创建了两个异步任务：`dummy_task`和`main_task`。`dummy_task`是`DummyEngine`的运行任务，而`main_task`是`PlanEngine`的运行任务，它接收初始节点`inital_node`和执行引擎`execution_engine`作为参数。

使用`asyncio.wait`等待这两个任务中的任何一个完成。一旦有任务完成，`asyncio.wait`会返回已完成的任务集合（`done`）和未完成的任务集合（`pending`）。如果`main_task`仍在`pending`集合中，说明它还未完成，此时会调用`task.cancel`来取消该任务，并打印出"Main task cancelled"消息。

**注意**：
- 该函数使用了`asyncio`库来实现异步编程，确保在使用时已经熟悉`asyncio`的基本概念和用法。
- `CONFIG`变量需要在函数外部定义，包含了引擎运行所需的配置信息。
- `set_interrupt`函数和`INTERRUPT`变量用于控制和传递中断信号，需要确保它们在XAgent框架中正确实现。
- 该函数中使用了`input`函数来获取用户输入，这意味着在异步环境中可能会阻塞事件循环，实际使用时应考虑替代方案。

**输出示例**：
由于该函数没有返回值，输出示例主要是控制台上可能出现的打印信息。例如：
```
Set Interrupt

Please input interrupt message: 用户输入的中断信息
Main task cancelled
```
## ClassDef DummyEngine
**DummyEngine 功能**: 该类的功能是模拟一个异步运行的引擎，用于测试中断处理。

DummyEngine 是一个继承自 BaseEngine 的类，它重写了 run 方法来实现其特定的逻辑。在 run 方法中，首先调用了 self.lazy_init 方法来进行一些可能的初始化操作，这通常是异步引擎在开始执行任务前的准备工作。初始化后，DummyEngine 会先等待20秒钟，然后打印一条消息 "\nSet Interrupt\n"，表示将要设置中断。

接下来，DummyEngine 调用 set_interrupt 函数来设置中断状态。这个函数是从 XAgent.interrput 模块中导入的，它的作用是触发一个中断，这通常用于告诉系统需要暂停或停止当前的操作。在设置中断后，DummyEngine 再次等待30秒钟，然后通过 input 函数提示用户输入一个中断消息。用户输入的消息将通过 INTERRUPT.set_message 方法设置到中断状态中，这个 INTERRUPT 是一个全局的中断状态对象。

在 sample/interrupt_2.py 文件中，DummyEngine 被用于创建一个异步任务 dummy_task。这个任务与主任务 main_task（由 PlanEngine 的 run 方法执行）并行运行。当 dummy_task 或 main_task 中的任何一个首先完成时，asyncio.wait 函数将返回，然后取消尚未完成的任务。如果主任务 main_task 未完成，它将被取消，并打印 "Main task cancelled" 消息。

**注意**:
- DummyEngine 类主要用于测试和演示中断机制的工作方式，它通过 asyncio.sleep 模拟长时间运行的任务，并在适当的时机触发中断。
- 在实际应用中，中断机制可以用于处理用户取消操作、超时或其他需要停止当前执行流程的场景。
- 使用 DummyEngine 时，需要注意它是一个异步类，它的 run 方法是一个协程，因此在调用时需要使用 asyncio.create_task 或其他异步调度方式来运行。
- 在使用 input 函数获取用户输入时，需要注意这将阻塞当前的事件循环，直到用户输入完成。在生产环境中，应避免在异步代码中直接使用 input，而是使用异步的输入方式。
### AsyncFunctionDef run
**run函数**: 该函数的功能是执行异步初始化，设置中断，并接收用户输入的中断信息。

该`run`函数是一个异步方法，属于`DummyEngine`类，它继承自`BaseEngine`。在`sample/interrupt_2.py`文件中，`DummyEngine`被用于模拟一个执行引擎的行为。以下是对该函数的详细分析：

1. `await self.lazy_init(self.config)`：这一行代码表示异步执行`lazy_init`方法，该方法通常用于执行引擎的初始化工作。这里传入的`self.config`是引擎的配置信息。

2. `await asyncio.sleep(delay=20)`：这一行代码使得当前异步任务暂停20秒。这通常用于模拟某些耗时的操作，或者在实际应用中用于等待某些条件成熟。

3. `print("\nSet Interrupt\n")`：打印信息，提示即将设置中断。

4. `await set_interrupt()`：这一行代码异步调用`set_interrupt`函数，该函数的作用是设置全局中断标志。在实际应用中，这可以用于通知其他正在执行的任务发生了中断事件。

5. `await asyncio.sleep(delay=30)`：再次使当前任务暂停30秒，等待用户输入中断信息。

6. `interrupt_msg = input("Please input interrupt message:")`：通过`input`函数接收用户输入的中断信息。

7. `INTERRUPT.set_message(interrupt_msg)`：将用户输入的中断信息设置到全局中断对象`INTERRUPT`中。这样，其他关注中断的任务就可以获取到这个信息。

在`sample/interrupt_2.py`文件中，`run`函数被用于创建一个名为`dummy_task`的异步任务。同时，还创建了另一个名为`main_task`的异步任务，它执行的是计划引擎`PlanEngine`的`run`方法。这两个任务被并行地执行，并且当任何一个任务完成时，`asyncio.wait`函数将返回。

如果`main_task`未完成，则会被取消。这模拟了一个场景，其中当中断发生时，主要的执行任务需要被取消。

**注意**：
- 由于`run`函数包含`input`函数调用，它将阻塞当前的事件循环直到用户输入完成。因此，这种方式在生产环境中的异步应用程序中并不常见，通常会有更好的异步接收用户输入的方式。
- 在使用`asyncio.sleep`进行延迟时，需要注意这会暂停当前协程的执行，但不会影响其他协程的运行。在实际应用中，应根据需要调整延迟时间。
- 在实际应用中，中断处理需要谨慎设计，确保所有相关的资源能够正确地响应中断信号，并进行适当的清理工作。
***
