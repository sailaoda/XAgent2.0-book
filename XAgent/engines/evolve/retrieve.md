# AsyncFunctionDef flatten_tree
**flatten_tree函数**: 该函数的功能是将包含嵌套子任务的树形结构展平，提取出所有的叶子节点，并移除这些叶子节点中的`tool_call_process`键。

该函数`flatten_tree`接收一个树形结构的根节点`root`作为参数。它的主要作用是遍历整个树形结构，将所有的叶子节点（即没有子任务的节点）收集到一个列表中。在这个过程中，它使用了`asyncio.gather`来并发地遍历子节点，这意味着该函数是异步执行的，适用于协程环境。

函数内部定义了一个异步的辅助函数`traverse`，用于递归地遍历树形结构。如果当前节点包含`"subtask"`键，表明它不是叶子节点，函数会递归地对其子节点进行遍历。如果当前节点不包含`"subtask"`键，表明它是叶子节点，函数会将其添加到`leaf_nodes`列表中。

在遍历完成后，函数会对收集到的叶子节点进行处理，移除其中的`"tool_call_process"`键。最终，函数返回一个包含所有叶子节点的列表。

在项目中，`flatten_tree`函数被用于处理从数据库检索到的树形结构数据。例如，在`change_db_retrieve_to_reference`和`refine_db_retrieve_to_reference`这两个函数中，它们首先从数据库中检索出与特定查询相似的句子，然后使用`flatten_tree`函数将检索结果中的树形结构展平，以便进一步处理这些叶子节点并构建参考计划。

**注意**：
- 由于`flatten_tree`函数是异步的，调用它时需要在异步环境中，并使用`await`关键字。
- 在处理实际的树形结构数据时，需要确保每个节点都是字典类型，并且叶子节点不包含`"subtask"`键。
- 在移除叶子节点中的`"tool_call_process"`键时，如果该键不存在于某些叶子节点中，`pop`方法不会引发错误，因为它会安静地忽略不存在的键。

**输出示例**：
假设有如下的树形结构数据：
```json
{
  "name": "root",
  "subtask": [
    {
      "name": "child1",
      "tool_call_process": "some_process",
      "subtask": [
        {
          "name": "grandchild1",
          "tool_call_process": "another_process"
        }
      ]
    },
    {
      "name": "child2"
    }
  ]
}
```
调用`flatten_tree`函数后，可能得到的返回值如下：
```json
[
  {
    "name": "grandchild1"
  },
  {
    "name": "child2"
  }
]
```
注意，返回的列表中包含了所有叶子节点，并且已经移除了`"tool_call_process"`键。
## AsyncFunctionDef traverse
**traverse函数**: 该函数的功能是遍历一个任务节点树，并收集所有叶子节点。

详细代码分析与描述：
`traverse` 函数是一个异步函数，它的目的是遍历一个表示任务的节点树结构，并且收集所有的叶子节点。在这个上下文中，一个叶子节点是指没有子任务（subtask）的节点。

函数接收一个参数 `node`，这个参数代表当前正在遍历的节点。

函数的逻辑如下：
1. 首先检查当前节点 `node` 是否包含键 `"subtask"`。如果包含，说明当前节点不是叶子节点，而是一个中间节点，它有子任务需要进一步遍历。
2. 如果当前节点是中间节点，函数将使用 `asyncio.gather` 来并发地遍历所有的子任务节点。`asyncio.gather` 是Python的异步并发执行机制，它可以等待一个由多个协程对象组成的列表，并发地执行它们。
3. 对于每一个子任务节点，函数会递归地调用自身 `traverse(subtask_node)` 来进行遍历。
4. 如果当前节点不包含 `"subtask"` 键，说明它是一个叶子节点。函数将这个叶子节点添加到一个名为 `leaf_nodes` 的列表中。

在 `flatten_tree` 函数中调用 `traverse` 函数时，`leaf_nodes` 被定义为一个空列表，用于收集所有叶子节点。遍历完成后，`flatten_tree` 函数会对收集到的叶子节点进行进一步的处理，例如移除不需要的键。

**注意**：
- `traverse` 函数是设计为内部使用的，它依赖于外部定义的 `leaf_nodes` 列表来收集叶子节点。因此，在调用 `traverse` 函数之前，必须确保 `leaf_nodes` 已经被正确地初始化。
- 由于 `traverse` 函数使用了 `asyncio.gather`，调用它的环境需要支持异步操作。
- 在处理并发任务时，应当注意异常处理和资源管理，以确保程序的健壮性和正确性。
***
# AsyncFunctionDef change_db_retrieve_to_reference
**change_db_retrieve_to_reference函数**: 该函数的功能是从向量数据库中检索与查询语句相似的句子，并基于检索结果构建参考执行计划。

该函数接收三个参数：`vector_db`是实现了VectorDBInterface接口的向量数据库对象，用于执行相似句子的搜索；`query`是一个字符串，表示要在数据库中搜索的查询语句；`namespace`是一个字符串，指定了搜索操作的命名空间。

函数首先使用`vector_db.search_similar_sentences`方法搜索与`query`相似的句子。搜索结果中的第一个句子的相似度得分会被检查，如果得分低于0.8，则函数返回`None`，表示没有找到足够相似的句子。

如果找到了相似度足够高的句子，函数会解析该句子的值（假定为JSON格式），并调用`flatten_tree`函数将结果中的树状结构展平，获取所有叶节点。这些叶节点代表了执行计划中的子任务。

接下来，函数构建初始的参考执行计划`ref_plan`，包含查询语句和任务ID。然后，函数遍历所有子任务，为每个子任务创建一个子计划`sub_plan`，包含子任务的名称、目标、里程碑、任务ID、执行状态和建议。如果子任务执行成功，会给出相应的成功建议；如果子任务包含提交结果，会根据提交结果中的建议来设置子计划的建议。

最终，所有子计划会被添加到初始参考执行计划的`subtasks`字段中，函数返回这个参考执行计划的JSON字符串表示。

在项目中，`change_db_retrieve_to_reference`函数被`XAgent/engines/plan.py`文件中的`run`方法调用。在执行计划引擎的运行过程中，如果配置启用了计划检索功能，`run`方法会尝试调用`change_db_retrieve_to_reference`函数来检索历史相似的执行计划，并将检索到的计划更新到工作上下文中，以供后续的计划生成和执行过程参考。

**注意**：
- 在使用`change_db_retrieve_to_reference`函数时，需要确保传入的`vector_db`对象实现了`VectorDBInterface`接口，并且数据库中已经存储了相关的句子数据。
- 函数中的`flatten_tree`需要是一个能够正确展平树状结构的异步函数。
- 函数返回的是JSON格式的字符串，调用者需要根据实际情况解析这个字符串以获取参考执行计划。

**输出示例**：
```json
{
  "query": "查询的目标",
  "task_id": "1",
  "subtasks": [
    {
      "subtask name": "子任务1",
      "goal": {
        "goal": "子任务1的目标",
        "criticsim": "子任务1的评价"
      },
      "milestones": "子任务1的里程碑",
      "task_id": "1.1",
      "execute_status": "SUCCESS",
      "suggestion": "This subtask and milestones are resonable and could be successfully achieved. You can take this subtask as reference."
    },
    {
      "subtask name": "子任务2",
      "goal": {
        "goal": "子任务2的目标",
        "criticsim": "子任务2的评价"
      },
      "milestones": "子任务2的里程碑",
      "task_id": "1.2",
      "execute_status": "FAIL",
      "suggestion": "N/A"
    }
  ]
}
```
***
# AsyncFunctionDef refine_db_retrieve_to_reference
**refine_db_retrieve_to_reference函数**: 此函数的功能是从数据库中检索与给定目标相似的句子，并根据检索结果构建一个参考计划。

该函数`refine_db_retrieve_to_reference`接受两个参数：`vector_db`和`goal`。`vector_db`是一个实现了VectorDBInterface接口的对象，用于与向量数据库进行交互。`goal`是一个字符串，表示用户的查询目标。

函数首先调用`vector_db.search_similar_sentences`方法，搜索与`goal`相似的句子，搜索范围限定在"workflow1208"中。搜索结果保存在变量`res`中。

接下来，函数检查最相似句子的得分。如果得分低于0.8，表示数据库中没有找到足够相似的参考计划，函数将返回一条提示信息，建议用户按照两种典型方式进行计划的优化。

如果得分高于阈值，函数将继续处理搜索结果。它首先将结果中的元数据部分解析为JSON格式，并将其赋值给`res`。

然后，函数调用`flatten_tree`方法将结果中的树状结构展平，以获取所有叶节点的信息。这一步骤的具体实现未在代码中给出，但可以推测其目的是为了提取出所有的子任务。

函数构建了一个初始的参考计划`ref_plan`，包含参考任务的名称、目标和任务ID。

之后，函数遍历所有的子任务，并为每个子任务构建一个子计划。在构建过程中，函数会检查子任务的执行状态，并据此设置子计划的状态。如果子任务成功执行，子计划将包含一条建议，指出该子任务和里程碑是合理的，可以作为参考。如果子任务包含提交结果，子计划还会包含对后续子任务计划的建议。

最后，函数将所有子计划作为参考添加到初始参考计划中，并将整个参考计划转换为JSON格式的字符串返回。

**注意**：
- 在使用此函数时，需要确保`vector_db`对象已正确实现了VectorDBInterface接口，并且能够与向量数据库进行交互。
- 函数中的`flatten_tree`方法需要能够正确处理树状结构的数据，并提取出所有叶节点的信息。
- 返回的参考计划是JSON格式的字符串，需要根据实际情况进行解析和使用。

**输出示例**：
```json
{
  "reference_task_name": "示例任务名称",
  "reference_task_goal": "示例任务目标",
  "task_id": "1",
  "subtasks": [
    {
      "subtask name": "子任务1",
      "goal": {
        "goal": "子任务1目标",
        "criticsim": "子任务1评价"
      },
      "milestones": ["里程碑1", "里程碑2"],
      "task_id": "1.1",
      "execute_status": "SUCCESS",
      "suggestion": "This subtask and milestones are resonable and could be successfully achieved. You can take this subtask as reference."
    },
    {
      "subtask name": "子任务2",
      "goal": {
        "goal": "子任务2目标",
        "criticsim": "子任务2评价"
      },
      "milestones": ["里程碑3", "里程碑4"],
      "task_id": "1.2",
      "execute_status": "FAIL",
      "suggestion": "N/A"
    }
  ]
}
```
在这个示例中，参考计划包含了一个主任务和两个子任务，每个子任务都有自己的目标、评价、里程碑、任务ID、执行状态和建议。
***
