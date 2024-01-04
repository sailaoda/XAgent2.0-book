# ClassDef TaskNode
**TaskNode函数**: 这个类的功能是表示一个任务节点。

TaskNode类继承自ExecutionNode和Task类。它是表示一个任务节点的对象。任务节点是执行图中的一个节点，用于表示一个具体的任务。该类没有定义任何属性或方法，只是作为一个基类存在。

**注意**: 在使用TaskNode类时，需要注意以下几点：
- TaskNode类是一个基类，用于表示任务节点。如果需要使用具体的任务节点，可以创建一个继承自TaskNode的子类，并在子类中定义具体的属性和方法。
- TaskNode类本身没有定义任何属性或方法，因此在使用时需要根据具体的需求进行扩展和实现。
***
# ClassDef ReActExecutionNode
**ReActExecutionNode函数**：这个类的功能是创建一个ReActExecutionNode对象。

ReActExecutionNode是ExecutionNode的子类，它继承了ExecutionNode的所有属性和方法。ExecutionNode是一个抽象基类，用于表示执行节点。ReActExecutionNode类没有定义任何自己的属性或方法，因此它只是一个空的子类。

注意：在使用ReActExecutionNode类时，需要先实例化一个对象。这个类没有定义任何特定的行为或功能，它只是作为ExecutionNode类的一个子类存在。
***
# ClassDef ReActExecutionGraph
**ReActExecutionGraph类的功能**：该类用于表示ReAct引擎的执行图。

ReActExecutionGraph类继承自ExecutionGraph类，并具有一个布尔类型的属性need_plan_rectification。

该类在项目中的调用情况如下：

1. 在XAgent/engines/evolve/execution_consolidate.py文件中的extract_pipeline函数中被调用。该函数用于提取执行图中的流水线信息，并返回流水线和追踪信息。在该函数中，通过调用exec_track的nodes属性获取执行图中的节点信息，并根据节点信息构建流水线和追踪信息。

2. 在XAgent/engines/evolve/execution_consolidate.py文件中的run函数中被调用。该函数用于执行任务并提取流水线和追踪信息。在该函数中，通过调用extract_pipeline函数提取流水线和追踪信息，并根据任务的结果更新执行图的状态。

3. 在XAgent/engines/pipeline.py文件中的run函数中被调用。该函数用于执行流水线任务。在该函数中，通过调用exec_track的nodes属性获取执行图中的节点信息，并根据节点信息执行相应的操作。

4. 在XAgent/engines/react.py文件中的__init__函数中被调用。该函数用于初始化ReAct引擎的执行图。

5. 在XAgent/engines/react.py文件中的run函数中被调用。该函数用于执行ReAct引擎并返回结果节点。在该函数中，通过调用step函数执行任务，并根据任务的结果更新执行图的状态。

6. 在XAgent/engines/react.py文件中的run函数中被调用。该函数用于执行ReAct引擎并返回结果节点。在该函数中，通过调用step函数执行任务，并根据任务的结果更新执行图的状态。

7. 在XAgent/engines/utils/insert_inner_records.py文件中的extract_pipeline函数中被调用。该函数用于提取执行图中的流水线信息，并返回流水线。在该函数中，通过调用exec_track的nodes属性获取执行图中的节点信息，并根据节点信息构建流水线。

8. 在XAgent/engines/utils/pipeline_extractor.py文件中的extract_pipeline函数中被调用。该函数用于提取执行图中的流水线信息，并返回流水线。在该函数中，通过调用exec_track的nodes属性获取执行图中的节点信息，并根据节点信息构建流水线。

**注意**：在使用ReActExecutionGraph类时，需要注意其属性need_plan_rectification的使用。
***
# ClassDef PlanExecutionNode
**PlanExecutionNode函数**: 这个类的功能是表示计划执行节点。

PlanExecutionNode类是TaskNode类的子类，它具有一个可选的actions属性，用于存储工具调用的列表。

在XAgent/engines/plan.py文件中的以下代码中，PlanExecutionNode类被实例化并使用：

```python
self.exec_node = PlanExecutionNode(
    actions=actions,
    **handling_task.dump_task(),
)
```

在这个代码片段中，PlanExecutionNode类被用于创建一个执行节点，其中包含了工具调用的列表和处理任务的其他属性。

在XAgent/engines/plan.py文件中的以下代码中，PlanExecutionNode类被作为函数的返回值使用：

```python
async def step(self,
               handling_task: PlanNode,
               execution_engine: BaseEngine,
               **kwargs) -> PlanExecutionNode:
    """Step and return execution result."""
    # ...
    self.exec_node = PlanExecutionNode(
        actions=actions,
        **handling_task.dump_task(),
    )
    # ...
    return self.exec_node
```

在这个代码片段中，step函数返回一个PlanExecutionNode对象，该对象包含了执行结果的相关信息。

在XAgent/engines/plan.py文件中的以下代码中，PlanExecutionNode类被作为函数的参数使用：

```python
async def plan_rectify(self,
                       handling_task: PlanNode,
                       interrupt_message: str = "",) -> PlanExecutionNode:
    # ...
    nnode = await self.step(
        handling_task=handling_task,
        execution_engine=execution_engine,
        **kwargs)
    # ...
```

在这个代码片段中，plan_rectify函数调用step函数，并将返回的PlanExecutionNode对象赋值给nnode变量。

**注意**: PlanExecutionNode类用于表示计划执行节点，并在计划引擎中的执行过程中使用。它包含了工具调用的列表和处理任务的其他属性。
***
# ClassDef PipelineTaskNode
**PipelineTaskNode函数**: 这个类的功能是定义了一个Pipeline任务节点，用于表示一个Pipeline任务的相关信息。

PipelineTaskNode类具有以下属性：
- pipeline_dir: 表示Pipeline文件所在的目录路径。
- overall_task_description: 表示整个Pipeline任务的描述信息。
- pipeline_input_params: 表示Pipeline任务的输入参数。

该类被以下文件调用：
文件路径：XAgent/engines/pipeline.py
调用部分代码如下：
    async def run(self, task: PipelineTaskNode, **kwargs) -> ExecutionGraph:
        """传入pipeline文件，执行pipeline"""
        await self.lazy_init(self.config)

        pipeline_dir = task.pipeline_dir
        self.query = task.overall_task_description
        self.exec_track = ReActExecutionGraph()
        self.exec_track.set_begin_node(task)
        self.last_exec_node = task

        self.load_tool_schemas(pipeline_dir)

        await self.route_agent.update_longterm_context(
            available_tools=self.tools_schema,
            current_goal=task.goal,
        )

        file_archi = await self.get_file_system_structure()
        observation = ""
        await self.route_agent.update_working_context(
            basic_info={
                "File System Structure": file_archi,
            },
            observation=observation,
        )

        recorder.log("Running pipeline: ", Fore.CYAN, str(pipeline_dir))

        dsl_code_module_relative_path = os.path.join(pipeline_dir,"decompiled_python_DSL_for_runner")
        dsl_code_module_relative_path = dsl_code_module_relative_path.replace("/",".")
        pipeline_dsl_module = importlib.import_module(dsl_code_module_relative_path)

        with open(os.path.join(pipeline_dir,"decompiled_python_DSL.py")) as reader:
            raw_code = "\n".join(reader.readlines())

        dsl_lookup_table_path = os.path.join(pipeline_dir,"decompiled_python_lookup_table.json")
        with open(dsl_lookup_table_path, "r", encoding="utf-8") as reader:
            lookup_table = json.load(reader)

        for ai_function_name, lookup_item in lookup_table.items():
            partial_func = partial(self.AI_route_and_give_params, code_str=raw_code, lookup_item=lookup_item)
            setattr(pipeline_dsl_module, ai_function_name, partial_func)

        setattr(pipeline_dsl_module, "transparent_function", partial(anonymous_class,executor=self.run_action))


        assert hasattr(pipeline_dsl_module, "mainWorkflow")
                # 获取函数并将其绑定为实例方法
        mainWorkflow = getattr(pipeline_dsl_module, "mainWorkflow")
        await mainWorkflow(mainWorkflow_input_data=None)
        self.last_exec_node.end_node = True
        self.exec_track.set_end_node(self.last_exec_node)

        task.conclusion = await self.route_agent.get_task_conclusion(task.goal)
        log_task_conclusion(conclusion=task.conclusion)
        self.exec_track.status = EngineExecutionStatusCode.SUCCESS if task.conclusion.status == "success" else EngineExecutionStatusCode.FAIL

        return self.exec_track
[代码片段结束]
[文件XAgent/engines/pipeline.py结束]
文件路径：XAgent/engines/plan.py
调用部分代码如下：
    async def step(self,
                   handling_task: PlanNode,
                   execution_engine: BaseEngine,
                   **kwargs) -> PlanExecutionNode:
        """Step and return execution result."""
        handling_task.status = TaskStatusCode.DOING

        recorder.log(
            f"-=-=-=-=-=-=-= Performing Task {handling_task.pid} ({handling_task.goal}): Begin -=-=-=-=-=-=-=",
            Fore.GREEN,
        )
        

        # Start listening to interruptions in the background
        self.handling_task = handling_task
        self.during_exec = True
        
        execution_engine.general_query = self.general_query # for self evolving
        if isinstance(execution_engine, PipelineEngine):
            pipeline_dir = None
            candidates = await retrieve_pipeline(handling_task, vector_db, namespace=self.config.execution.evolve.inner_namespace)
            for candidate in candidates:
                retrieved_pipeline = candidate["value_sentence"]
                score = candidate["score"]
                # if retrieves non exist pipeline name, skip to next candidate
                if not os.path.exists(os.path.join(self.config.execution.pipeline.data_root, retrieved_pipeline, "decompiled_python_DSL_for_runner.py")):
                    continue
                pipeline_dir = os.path.join(self.config.execution.pipeline.data_root, retrieved_pipeline)
                break
            
            exec_result = await execution_engine.run(
                task = PipelineTaskNode(
                    pipeline_dir=pipeline_dir,
                    overall_task_description=handling_task.goal,
                    pipeline_input_params={}
                ),
                **kwargs
            )
                
        else:
          exec_result = await execution_engine.run(
              task=handling_task,
              plan_tree=self.exec_track.plan_tree,
              # Newly added, to enable set plan interruption in the react execution
              plan_interruption_vec=self.interrupt.vector,
              **kwargs)

        actions = []
        for node in exec_result.get_execution_track():
            actions.extend(node.tool_calls)

        self.exec_node = PlanExecutionNode(
            actions=actions,
            **handling_task.dump_task(),
        )
        handling_task.status = TaskStatusCode.SUCCESS if exec_result.status == EngineExecutionStatusCode.SUCCESS else TaskStatusCode.FAIL

        recorder.log(
            f"-=-=-=-=-=-=-= Task {handling_task.pid} ({handling_task.goal}): {handling_task.status.value} -=-=-=-=-=-=-=",
            handling_task.status.color(),
        )
        
        # waiting for the interruption handling to stop to ensuring rectify one at a time
        while self.interrupt_during_exec:
            await asyncio.sleep(0.5)
        # Stop listening to interruptions when the execution is done
        self.during_exec = False

        # Incorporate all the interruptions from buffer finally to th node
        if len(self.interrupt_msg_buffers) != 0:
            self.exec_node.interrupts.extend(self.interrupt_msg_buffers)
            self.interrupt_msg_buffers = []
        
        # We only handle the true plan rectify here. All the interruption handle go to the listener
        if getattr(exec_result, 'need_plan_rectification', False):
            await self.plan_rectify(
                handling_task=handling_task,
            )

        remaning_task: list[PlanNode] = self.exec_track.plan_tree.get_remining()
        self.exec_node.end_node = len(remaning_task) == 0

        return self.exec_node

[代码片段结束]
[文件XAgent/engines/plan.py结束]
文件路径：XAgent/workflow/pipeline.py
调用部分代码如下：
    async def run(self,
                  pipeline_dir: str,
                  goal: str,
                  pipeline_input_params: dict):
        from XAgent.config import CONFIG
        from XAgent.models import PipelineTaskNode
        from XAgent.engines import PipelineEngine
        from XAgent.record import recorder
        await recorder.save_query(query=goal)
        pipeline = PipelineEngine(CONFIG)
        pipeline.general_query = goal
        ret = await pipeline.run(
            task = PipelineTaskNode(
                pipeline_dir=pipeline_dir,
                overall_task_description=goal,
                pipeline_input_params=pipeline_input_params
            )
        )
        return ret
[代码片段结束]
[文件XAgent/workflow/pipeline.py结束]
调用部分代码如下：
    async def run(self,
                  pipeline_dir: str,
                  goal: str,
                  pipeline_input_params: dict):
        from XAgent.config import CONFIG
        from XAgent.models import PipelineTaskNode
        from XAgent.engines import PipelineEngine
        from XAgent.record import recorder
        await recorder.save_query(query=goal)
        pipeline = PipelineEngine(CONFIG)
        pipeline.general_query = goal
        ret = await pipeline.run(
            task = PipelineTaskNode(
                pipeline_dir=pipeline_dir,
                overall_task_description=goal,
                pipeline_input_params=pipeline_input_params
            )
        )
        return ret
[代码片段结束]
[文件XAgent/workflow/pipeline.py结束]

**注意**: 该类表示一个Pipeline任务节点，用于表示一个Pipeline任务的相关信息。在执行Pipeline任务时，会根据传入的Pipeline文件路径、任务描述和输入参数创建一个PipelineTaskNode对象，并将其作为参数传递给PipelineEngine的run方法进行执行。在执行过程中，会根据Pipeline文件的内容进行相应的处理和调用。


***
# ClassDef PlanExecutionGraph
**PlanExecutionGraph函数**：这个类的功能是创建一个执行计划图。

该类继承自ExecutionGraph类，并具有一个plan_tree属性，表示执行计划的树形结构。

**代码分析和描述**：
- plan_tree: PlanTree类型的属性，表示执行计划的树形结构。在执行计划图中，根节点是任务的起始节点，每个节点表示一个任务。通过该属性，可以获取执行计划的整体结构。

**注意**：在使用该类时需要注意以下几点：
- 在创建PlanExecutionGraph对象时，需要传入一个PlanTree对象作为参数，用于构建执行计划的树形结构。
- 可以通过访问plan_tree属性来获取执行计划的树形结构。
- 该类继承自ExecutionGraph类，因此可以使用ExecutionGraph类中的方法和属性。
- 在使用该类时，需要确保传入的plan_tree参数是一个有效的PlanTree对象，否则可能会导致执行计划图的构建失败。
***
