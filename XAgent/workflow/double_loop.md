# ClassDef DoubleLoopFlow
**DoubleLoopFlow函数**：这个类的功能是运行一个双重循环的流程。

在XAgentServer/server.py文件中，我们可以看到DoubleLoopFlow类被调用并启动了一个双重循环的流程。在这个流程中，首先从XAgent.config模块导入CONFIG对象和PlanNode类，然后从XAgent.engines模块导入ReActEngine和PlanEngine类，从XAgent.record模块导入recorder对象。接下来，调用recorder对象的save_query方法保存查询的目标。然后，创建一个PlanEngine对象，并传入一个PlanNode对象作为参数，其中包含了流程的名称、目标和里程碑。然后，创建一个ReActEngine对象，并将PlanEngine对象作为参数传入。最后，调用PlanEngine对象的run方法，并返回运行结果。

在main.py文件中，我们可以看到DoubleLoopFlow类被调用并启动了一个双重循环的流程。在这个流程中，首先从XAgent.config模块导入CONFIG对象和ARGS对象，然后从XAgent模块导入init_db函数。接下来，初始化数据库，并根据传入的参数设置配置。然后，创建一个ToolServerInterface对象，并调用其lazy_init方法进行初始化。接着，根据传入的参数上传文件。最后，根据传入的参数选择不同的流程进行启动。

在tests/test_pipeline.py文件中，我们可以看到DoubleLoopFlow类被调用并启动了一个双重循环的流程。在这个流程中，首先从XAgent.config模块导入CONFIG对象和ARGS对象，然后从XAgent模块导入init_db函数。接下来，根据传入的参数设置配置。然后，根据传入的参数上传文件。接着，初始化数据库，并根据传入的参数设置配置。然后，根据传入的参数选择不同的流程进行启动。

在tests/test_plan_pipeline_loop.py文件中，我们可以看到DoubleLoopFlow类被调用并启动了一个双重循环的流程。在这个流程中，首先从XAgent.config模块导入CONFIG对象和ARGS对象，然后从XAgent模块导入init_db函数。接下来，根据传入的参数设置配置。然后，根据传入的参数上传文件。接着，初始化数据库，并根据传入的参数设置配置。然后，根据传入的参数选择不同的流程进行启动。

在tests/test_self_evolve_consolidation.py文件中，我们可以看到DoubleLoopFlow类被调用并启动了一个双重循环的流程。在这个流程中，首先从XAgent.config模块导入CONFIG对象和ARGS对象，然后从XAgent模块导入init_db函数。接下来，根据传入的参数设置配置。然后，根据传入的参数上传文件。接着，初始化数据库，并根据传入的参数设置配置。然后，根据传入的参数选择不同的流程进行启动。

**注意**：在使用这段代码时需要注意以下几点：
- 在使用DoubleLoopFlow类之前，需要先导入相关的模块和类。
- 在调用DoubleLoopFlow类的start方法时，需要传入目标和里程碑等参数。

**输出示例**：模拟代码返回值的可能外观。
## AsyncFunctionDef run
**run函数**: 该函数的功能是执行一个基于用户设定目标的规划和响应循环。

该`run`函数是一个异步函数，它接受两个参数：`goal`和`milestones`。`goal`是一个字符串，表示用户设定的目标；`milestones`是一个字符串列表，默认为空，表示在实现目标过程中的里程碑。

函数的主要步骤如下：

1. 首先，函数从`XAgent.config`模块导入`CONFIG`配置对象，从`XAgent.models`模块导入`PlanNode`类，以及从`XAgent.engines`模块导入`ReActEngine`和`PlanEngine`两个引擎类。

2. 接着，函数使用`recorder`对象的`save_query`方法异步保存用户的查询目标`goal`。

3. 然后，函数创建一个`PlanEngine`实例，传入配置对象`CONFIG`。

4. 同时，创建一个`ReActEngine`实例，也传入配置对象`CONFIG`。

5. 接下来，函数调用`PlanEngine`实例的`run`方法，传入一个`PlanNode`实例和`ReActEngine`实例。`PlanNode`实例包含了任务名称、用户设定的目标`goal`和里程碑`milestones`。

6. `PlanEngine`的`run`方法将执行规划过程，并使用`ReActEngine`作为执行引擎来处理规划结果。

7. 最后，函数返回`PlanEngine`的`run`方法的结果。

**注意**：
- 由于`run`函数是异步的，调用它时需要使用`await`关键字。
- 在使用此函数之前，确保已经正确配置了`XAgent.config`中的`CONFIG`对象。
- 函数返回的结果取决于`PlanEngine`的`run`方法的实现，可能是一个表示执行结果的对象或其他数据结构。

**输出示例**：
假设`PlanEngine`的`run`方法返回一个字典，包含规划和执行的结果，那么`run`函数的返回值可能如下所示：
```python
{
    "status": "success",
    "details": {
        "plan": "Generated plan based on the goal",
        "execution": "Execution details after running the plan"
    }
}
```
这个字典包含了执行状态和一个包含规划和执行细节的子字典。
***
