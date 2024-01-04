# FunctionDef flatten_tree
**flatten_tree函数**: 该函数的作用是将一个具有层级结构的任务树展平，提取出所有的叶子节点。

该函数接收一个根节点`root`作为参数，这个根节点代表了一个任务树的顶层结构。函数内部定义了一个名为`traverse`的递归辅助函数，用于遍历整个任务树。如果当前节点包含`"subtask"`键，表示它有子任务，函数将递归地对每个子任务调用`traverse`函数。如果当前节点不包含`"subtask"`键，说明它是一个叶子节点，将其添加到`leaf_nodes`列表中。

在遍历完所有节点后，函数会对`leaf_nodes`列表中的每个叶子节点进行处理，移除其中的`"tool_call_process"`键（如果存在）。最后，函数返回包含所有叶子节点的列表`leaf_nodes`。

在项目中，`flatten_tree`函数被调用于`XAgent/engines/utils/retrieve.py`文件中的`change_db_retrieve_to_reference`和`refine_db_retrieve_to_reference`两个函数中。这两个函数都是处理数据库检索结果的函数，它们使用`flatten_tree`来将检索到的具有层级结构的结果展平，以便进一步处理每个叶子节点的数据。

**注意**：
- 在使用`flatten_tree`函数时，需要确保传入的`root`参数具有正确的层级结构，即至少包含`"subtask"`键或者是一个叶子节点。
- 函数会修改传入的任务树结构，移除所有叶子节点中的`"tool_call_process"`键，因此如果原始数据中这些信息是必要的，需要在调用函数前做好备份。

**输出示例**：
假设有如下的任务树结构：

```json
{
  "name": "主任务",
  "subtask": [
    {
      "name": "子任务1",
      "subtask": [
        {
          "name": "叶子任务1",
          "tool_call_process": "调用工具A的过程"
        }
      ]
    },
    {
      "name": "叶子任务2",
      "tool_call_process": "调用工具B的过程"
    }
  ]
}
```

调用`flatten_tree`函数后，返回的结果将是：

```json
[
  {
    "name": "叶子任务1"
  },
  {
    "name": "叶子任务2"
  }
]
```

可以看到，原始任务树中的所有叶子节点都被提取出来，并且它们的`"tool_call_process"`键已经被移除。
## FunctionDef traverse
**traverse函数**: 该函数的功能是遍历一个表示任务结构的树形数据结构，并收集所有的叶子节点。

该`traverse`函数是一个递归函数，它的目的是遍历一个树形结构，这个结构中的每个节点可能包含一个名为`"subtask"`的字段，该字段是一个列表，列表中包含了该节点的子任务。如果一个节点包含`"subtask"`字段，那么这个节点被视为一个中间节点；如果不包含，那么它就是一个叶子节点。

在函数的实现中，首先检查传入的节点`node`是否包含`"subtask"`字段。如果包含，函数会遍历`"subtask"`列表中的每个子节点，并对每个子节点递归调用`traverse`函数。如果当前节点不包含`"subtask"`字段，说明它是一个叶子节点，函数会将这个节点添加到全局变量`leaf_nodes`列表中。

需要注意的是，`leaf_nodes`应该在`traverse`函数外部定义，并且在开始遍历之前应该是一个空列表。这样，每次遍历开始时，`leaf_nodes`都会被初始化，以便收集新的叶子节点。

在`flatten_tree`函数中，`traverse`函数被用来遍历树的根节点`root`，并收集所有的叶子节点到`leaf_nodes`列表中。在遍历结束后，`flatten_tree`函数会进一步处理`leaf_nodes`列表中的每个叶子节点，例如移除不需要的字段。

**注意**:
- `traverse`函数依赖于外部定义的`leaf_nodes`列表来收集叶子节点，因此在调用`traverse`之前，需要确保`leaf_nodes`已经被正确初始化。
- 由于`traverse`函数是递归调用的，所以它适用于深度不会导致栈溢出的树形结构。
- 在`flatten_tree`函数中，对`leaf_nodes`的进一步处理应根据实际需求来定，例如在示例中移除了`"tool_call_process"`字段。
- `traverse`函数没有返回值，它通过修改外部变量`leaf_nodes`来传递结果。
***
# FunctionDef change_db_retrieve_to_reference
**change_db_retrieve_to_reference函数**：该函数的功能是将数据库检索到的查询结果转换为参考计划的格式。

该函数首先通过`VectorDBInterface.search_similar_sentences`函数使用提供的查询（`query`）在数据库中搜索相似的句子，搜索范围限定为"whole_workflow"。搜索结果是一个JSON格式的字符串，其中包含了匹配的元数据。函数随后将这个JSON字符串解析为Python字典，并取出匹配结果的第一项的元数据中的"text"字段。

接下来，函数使用`flatten_tree`函数将结果中的树状结构扁平化，以获取所有叶节点的信息。这一步骤的目的是为了提取出所有子任务的详细信息。然后，函数构造了一个名为`ref_plan`的字典，其中包含了原始查询的目标（`res["goal"]`）和一个固定的任务ID（"1"）。

函数继续处理子任务，为每个子任务创建一个子计划（`sub_plan`），并将其添加到子计划列表（`sub_plans`）中。每个子计划包含子任务的名称、目标、批评、里程碑、任务ID、执行状态和建议。执行状态根据子任务的`exceute_status`字段来确定，如果该字段为"SPLIT"则状态为"FAIL"，否则保持原状态。如果状态不是"SUCCESS"或"FAIL"，则将其设置为"UNKNOWN"。如果子任务成功执行，建议字段会包含一条消息，表明这个子任务和里程碑是合理的，可以作为参考。如果子任务包含提交结果，建议字段会包含提交结果中的建议。

最后，函数将所有子计划作为参考添加到`ref_plan`字典的"subtasks"键下，并将该字典转换为格式化的JSON字符串返回。

**注意**：
- 该函数目前是同步执行的，根据TODO注释，未来可能会改为异步执行。
- 在处理子任务的执行状态时，存在一个拼写错误："exceute_status"应该是"execute_status"。
- 函数返回的是一个JSON格式的字符串，如果需要在Python中使用，可能需要再次解析。

**输出示例**：
```json
{
  "query": "查询的目标",
  "task_id": "1",
  "subtasks": [
    {
      "subtask name": "子任务名称",
      "goal": {
        "goal": "子任务目标",
        "criticsim": "子任务批评"
      },
      "milestones": "子任务里程碑",
      "task_id": "1.1",
      "execute_status": "SUCCESS",
      "suggestion": "This subtask and milestones are resonable and could be successfully achieved. You can take this subtask as reference."
    },
    ...
  ]
}
```
以上JSON字符串展示了一个可能的返回值示例，其中包含了查询目标、任务ID和一个子任务列表。每个子任务都有自己的名称、目标、批评、里程碑、任务ID、执行状态和建议。
***
# FunctionDef refine_db_retrieve_to_reference
**refine_db_retrieve_to_reference函数**：该函数的功能是将数据库检索到的任务结果进行精炼，生成参考计划。

该函数的具体代码如下：
```python
def refine_db_retrieve_to_reference(goal):# TODO: change to async
    res = vector_db_interface.search_similar_sentences(goal, "workflow")
    res = json.loads(res["matches"][0]["metadata"]["text"])

    # flatten the tree to get all the leaves
    wanted_res = flatten_tree(res)
    # Maintain the original tree but only use the first level
    # wanted_res = res["subtask"]

    ref_plan = {"reference_task_name": res["name"], "reference_task_goal": res["goal"], 'task_id': "1"}

    # begin to handle subtask (same process)
    sub_plans = []
    cur_id = 0
    for subtask in wanted_res:
        cur_id += 1
        # Typo in "execute"!
        status = "FAIL" if subtask["exceute_status"] == "SPLIT" else subtask["exceute_status"]
        status = "UNKNOWN" if status != "SUCCESS" and status != "FAIL" else status
        sub_plan = {"subtask name": subtask["name"], "goal": {"goal": subtask["goal"], "criticsim": subtask["prior_plan_criticsim"]}, "milestones": subtask["milestones"], "task_id": f"1.{cur_id}", "execute_status": status, "suggestion": "N/A"}
        if status == "SUCCESS":
            sub_plan["suggestion"] = "这个子任务和里程碑是合理的，可以成功完成。您可以将此子任务作为参考。"
        elif "submit_result" in subtask:
            sub_plan["suggestion"] = subtask["submit_result"]["args"]["suggestions_for_latter_subtasks_plan"]["reason"]
        sub_plans.append(sub_plan)
    # Add the subplans finally as reference
    ref_plan["subtasks"] = sub_plans

    return json.dumps(ref_plan, indent=2, ensure_ascii=False)
```

该函数接收一个目标参数`goal`，首先通过调用`vector_db_interface.search_similar_sentences`函数从数据库中检索与目标参数相似的任务结果。然后将检索到的结果进行解析，获取任务的名称、目标和子任务等信息。

接下来，函数将结果进行精炼处理。首先通过`flatten_tree`函数将任务结果的树形结构展开，获取所有叶子节点。然后根据需求，可以选择保留原始树的第一层子任务，或者将整个树展开。

接着，函数创建一个参考计划`ref_plan`，包括参考任务的名称、目标和任务ID等信息。

然后，函数开始处理子任务。遍历所有的子任务，为每个子任务创建一个子计划`sub_plan`。根据子任务的执行状态，将执行状态转换为统一的标识。如果子任务执行成功，则将建议设置为该子任务和里程碑是合理的，可以成功完成。如果子任务执行失败，则根据提交结果中的建议设置建议内容。

最后，将所有的子计划添加到参考计划中，并将参考计划转换为JSON格式返回。

**注意**：该函数中存在一个拼写错误，`exceute_status`应为`execute_status`。

**输出示例**：模拟函数返回值的可能外观。
```json
{
  "reference_task_name": "参考任务名称",
  "reference_task_goal": "参考任务目标",
  "task_id": "1",
  "subtasks": [
    {
      "subtask name": "子任务1",
      "goal": {
        "goal": "子任务1目标",
        "criticsim": "子任务1优先计划相似度"
      },
      "milestones": "子任务1里程碑",
      "task_id": "1.1",
      "execute_status": "执行状态",
      "suggestion": "建议"
    },
    {
      "subtask name": "子任务2",
      "goal": {
        "goal": "子任务2目标",
        "criticsim": "子任务2优先计划相似度"
      },
      "milestones": "子任务2里程碑",
      "task_id": "1.2",
      "execute_status": "执行状态",
      "suggestion": "建议"
    },
    ...
  ]
}
```

请注意：
- 生成的内容中不应包含Markdown的标题和分隔符语法。
- 文档主要使用中文撰写，如果需要，可以在分析和描述中使用一些英文词汇以增强文档的可读性，因为您不需要将函数名或变量名翻译为目标语言。
***
# FunctionDef get_additional_msg_from_retrieve
**get_additional_msg_from_retrieve 函数**: 该函数的功能是生成并返回一个包含失败任务总结、目标总结、参考计划和改进建议的消息对象。

该函数接收两个参数：`now_dealing_task` 和 `ultimate_goal`。`now_dealing_task` 是当前正在处理的任务节点对象，它应该是一个 `PlanNode` 类型，代表了一个计划中的任务节点。`ultimate_goal` 是一个表示最终目标的变量。

函数的主要步骤如下：

1. 首先，函数获取当前任务节点的目标（`cur_level_goal`）和父任务节点的目标（`father_level_goal`）。

2. 接着，函数调用 `refine_db_retrieve_to_reference` 函数（该函数未在代码中定义，可能是在其他部分定义）来获取与当前任务节点和父任务节点的目标相似的参考计划。

3. 函数从当前任务节点中提取失败任务的结论（`fail_conclusion`）、后续子任务计划的建议（`suggestion`）、任务ID（`fail_task_id`）、任务名称（`fail_task_name`）和任务目标（`fail_task_goal`）。

4. 然后，函数将失败任务的信息和建议整理成一个 JSON 格式的字符串 `summary`，并将当前目标、父目标和最终目标整理成另一个 JSON 格式的字符串 `goals`。

5. 最后，函数创建一个 `Message` 对象 `additional_message`，它包含了失败任务的总结、目标总结、参考计划和两种改进建议。这个消息对象是函数的返回值。

**注意**：
- 在使用此函数时，需要确保 `now_dealing_task` 参数是一个有效的 `PlanNode` 对象，并且它包含了所需的所有属性和方法。
- `ultimate_goal` 参数应该是一个清晰定义的最终目标。
- 函数中提到的 `refine_db_retrieve_to_reference` 函数需要在其他地方定义，且能够根据给定的目标返回相应的参考计划。
- 函数中的 `to_json` 方法假设 `PlanNode` 对象能够转换为 JSON 格式。
- 函数目前标记为 `TODO: change to async`，表明未来可能需要将其改写为异步函数以提高性能。

**输出示例**：
一个可能的返回值示例是一个 `Message` 对象，它包含了以下信息的字符串表示：
- 失败任务的 ID、名称、目标、结论和改进建议。
- 当前目标、父目标和最终目标的总结。
- 与当前目标和父目标相似的参考计划。
- 针对失败情况的两种改进建议。

返回的 `Message` 对象可能会被用于与用户交互，向用户展示失败任务的详细信息，并提供改进计划的建议。
***
