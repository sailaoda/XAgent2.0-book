# FunctionDef update_subdict
**update_subdict函数**: 此函数的功能是更新嵌套字典结构中特定ID的任务或子任务的信息。

update_subdict函数接收三个参数：`ori_dict`、`id`和`update_items`。其中，`ori_dict`是原始的字典列表，每个字典代表一个任务或子任务；`id`是需要更新的任务或子任务的唯一标识符；`update_items`是一个字典，包含了需要更新的键值对。

函数的执行逻辑如下：
1. 遍历`ori_dict`中的每个字典（代表一个任务或子任务）。
2. 如果当前字典中的`task_id`与参数`id`相匹配，那么就遍历`update_items`字典，并将其键值对更新到当前字典中。
3. 如果更新成功，函数返回更新后的`ori_dict`和布尔值`True`。
4. 如果当前字典中包含键`subtask`，表明存在子任务，函数将递归调用自身，传入子任务列表和相同的`id`与`update_items`。
5. 如果在任何子任务中找到匹配的`task_id`并完成更新，函数同样返回更新后的`ori_dict`和布尔值`True`。
6. 如果遍历完所有任务和子任务都没有找到匹配的`task_id`，则返回原始的`ori_dict`和布尔值`False`，表示没有进行任何更新。

**注意**：
- 确保传入的`ori_dict`具有正确的结构，即列表中的每个元素都是字典，并且每个字典都有`task_id`键。
- `update_items`中的键应该与`ori_dict`中字典的键相匹配，否则会添加新的键。
- 函数使用了递归调用来处理嵌套的子任务，因此要注意不要传入过深的嵌套结构，以避免栈溢出。

**输出示例**：
假设有以下`ori_dict`和`update_items`：
```python
ori_dict = [
    {"task_id": 1, "name": "Task A", "status": "pending"},
    {"task_id": 2, "name": "Task B", "status": "pending", "subtask": [
        {"task_id": 3, "name": "Subtask B1", "status": "pending"}
    ]}
]
update_items = {"status": "completed"}
```
调用`update_subdict(ori_dict, 2, update_items)`后，返回的结果可能如下：
```python
(
    [
        {"task_id": 1, "name": "Task A", "status": "pending"},
        {"task_id": 2, "name": "Task B", "status": "completed", "subtask": [
            {"task_id": 3, "name": "Subtask B1", "status": "pending"}
        ]}
    ],
    True
)
```
这表明ID为2的任务已经被更新为完成状态。
***
# FunctionDef dict_insert_vec
**dict_insert_vec函数**：该函数的作用是将字典列表中的每个字典元素转换为特定格式并插入到向量数据库中。

该函数`dict_insert_vec`接受一个字典列表`ori_dict`作为参数，遍历列表中的每个字典元素`cur_dict`。对于每个`cur_dict`，它会构造一个新的字典`insert_dict`，该字典包含以下键值对：
- "goal"：当前字典中的目标。
- "prior_plan_criticsim"：当前字典中的先前计划批判性分析。
- "milestones"：当前字典中的里程碑。
- "if_success_previously"：根据当前字典中的执行状态`status`，如果状态是"SUCCESS"或"FAIL"，则使用该状态，否则设置为"UNKNOWN"。
- "action_list_summary"：默认设置为"UNAVAILABLE"。

在尝试从`cur_dict`中获取"action_list_summary"键的值时，如果成功，则会打印"Valid summary presented!"并更新`insert_dict`中的"action_list_summary"；如果失败（即`cur_dict`中不存在该键），则会打印"Summary unavailable currently"。

之后，函数会调用`vecdb.insert_sentence`方法，将构造的`insert_dict`字典转换为JSON字符串，并与目标`goal`一起插入到向量数据库中，标记为"single_step_plan"类型。

如果当前字典`cur_dict`中存在"subtask"键，则递归调用`dict_insert_vec`函数，对其值（子任务列表）进行相同的处理。

**注意**：
- 在使用`dict_insert_vec`函数时，需要确保传入的`ori_dict`是一个字典列表，且每个字典都包含"goal"、"exceute_status"、"prior_plan_criticsim"和"milestones"键。
- 函数中使用了递归调用来处理嵌套的子任务，因此需要注意可能的最大递归深度限制。
- 函数依赖于外部的`vecdb.insert_sentence`方法和`json.dumps`函数，因此在使用前需要确保这些依赖项已正确配置和可用。
- 函数中的错误处理是通过打印信息到控制台来进行的，这可能不适合生产环境。在生产环境中，可能需要更健壮的错误处理机制，例如记录到日志文件或数据库。
- 函数假设`vecdb.insert_sentence`方法存在并且可以正常工作，如果向量数据库的接口有变化，可能需要对函数进行相应的修改。
***
# FunctionDef workflow_insert_vec
**workflow_insert_vec函数**: 该函数的功能是将工作流程中的每个子任务插入到向量数据库中。

该函数`workflow_insert_vec`接收一个字典列表`ori_dict`作为参数，该列表通常包含工作流程的结构信息。函数的目的是遍历这个列表，并将每个子任务的目标描述和结构信息插入到向量数据库中，以便于后续的检索和分析。

具体的代码分析如下：

1. 函数定义了一个变量`insert_num`用于记录插入数据库的次数。
2. 遍历传入的字典列表`ori_dict`，对于列表中的每个字典元素`cur_dict`：
   - 检查`cur_dict`是否包含键`"subtask"`和`"goal"`。如果不包含，则跳过当前的字典元素。
   - 如果包含这些键，那么将`cur_dict`中的`"goal"`字段值去除首尾空格后，连同字典的JSON字符串形式（通过`json.dumps`格式化，保证中文不会被转义），插入到向量数据库中。这里的`vecdb.insert_sentence`是一个假设存在的数据库插入方法，用于将数据插入到数据库中。
   - `insert_num`增加1，表示成功插入了一个记录。
   - 递归调用`workflow_insert_vec`函数，传入当前字典元素的`"subtask"`字段值，该字段值也是一个字典列表，代表当前任务的子任务。递归的结果（即子任务的插入次数）累加到`insert_num`上。
3. 函数返回`insert_num`，即总共插入到数据库中的记录数。

在项目中的调用情况：

- 在`insert_records.py`文件中，`workflow_insert_vec`函数被`post_process_plan_insert`函数调用。`post_process_plan_insert`函数处理一个完整的工作流程计划，并调用`workflow_insert_vec`来插入所有子工作流程到数据库中。
- `post_process_plan_insert`函数首先将整个工作流程计划插入到数据库中，然后调用`workflow_insert_vec`函数将所有子工作流程（不包括叶节点）插入到数据库中，并打印出插入的总数。

**注意**：
- 在实际使用中，需要确保`vecdb.insert_sentence`方法存在，并且能够正确地将数据插入到向量数据库中。
- 该函数使用了递归调用来处理嵌套的子任务，因此需要注意递归深度，以防止栈溢出。

**输出示例**：
假设有一个工作流程计划包含3个子任务，调用`workflow_insert_vec`函数后，可能的返回值为：

```plaintext
3
```

这表示函数成功将3个子任务插入到了向量数据库中。
***
# FunctionDef pop_tool_call_process
**pop_tool_call_process函数**：此函数的功能是从字典对象中移除键为"tool_call_process"的项，并递归地对所有子任务(subtask)执行相同的移除操作。

详细代码分析与描述：
`pop_tool_call_process`函数接受一个字典参数`ori_dict`，该字典代表一个任务或计划。函数的主要目的是清理这个字典对象，移除其中不需要的数据，以便于后续处理。

首先，函数检查`ori_dict`字典中是否存在键名为"tool_call_process"的项。如果存在，该函数会使用`del`语句将其删除。这一步骤可能是为了去除与工具调用相关的过程信息，这些信息在某些上下文中可能不再需要。

接下来，函数检查字典中是否有"subtask"这个键。如果有，说明这个任务包含子任务，函数将遍历每一个子任务，并递归地调用`pop_tool_call_process`函数，对每个子任务执行相同的清理操作。这确保了整个任务结构中的所有层级都被清理。

最后，函数返回清理后的字典对象`ori_dict`。

在项目中的调用情况：
在`XAgent/engines/utils/insert_records.py`文件中，`pop_tool_call_process`函数被用于预处理插入VecDB之前的计划(plan)。在插入整个工作流程到VecDB之前，需要移除计划中所有的工具调用信息，这可能是因为这些信息会导致存储时的令牌超出限制。函数的调用确保了计划中不包含工具调用过程信息，然后计划被插入到数据库中。

**注意**：
- 在调用此函数时，需要确保传入的字典对象包含了"tool_call_process"和"subtask"这两个键，否则函数将不会执行任何操作。
- 由于函数使用了递归，需要注意传入的任务结构不应该太深，以避免可能的栈溢出错误。

**输出示例**：
假设有一个任务字典如下：
```json
{
  "name": "Example Task",
  "tool_call_process": "Some process information",
  "subtask": [
    {
      "name": "Subtask 1",
      "tool_call_process": "Subtask process information"
    },
    {
      "name": "Subtask 2",
      "subtask": [
        {
          "name": "Sub-Subtask",
          "tool_call_process": "Sub-Subtask process information"
        }
      ]
    }
  ]
}
```
调用`pop_tool_call_process`函数后，返回的字典将不再包含任何"tool_call_process"键：
```json
{
  "name": "Example Task",
  "subtask": [
    {
      "name": "Subtask 1"
    },
    {
      "name": "Subtask 2",
      "subtask": [
        {
          "name": "Sub-Subtask"
        }
      ]
    }
  ]
}
```
***
# FunctionDef post_process_plan_insert
**post_process_plan_insert函数**：该函数的功能是将计划数据插入到VecDB数据库中。

该函数接收一个名为`plan`的参数，该参数是一个包含计划数据的字典。函数的处理流程如下：

1. 首先，调用`pop_tool_call_process`函数处理`plan`，以确保如果计划内容过长导致令牌超出限制时，能够先移除所有的工具调用。

2. 接着，将计划的名称（`name`）和目标（`goal`）拼接成一个字符串，并与计划的JSON格式字符串一起插入到VecDB数据库中。这里使用的是`vecdb.insert_sentence`方法，将整个工作流程作为记录插入数据库。插入成功后，会打印出成功信息。

3. 最后，函数会调用`workflow_insert_vec`函数，将计划中的所有子树（排除所有叶节点）插入到数据库中，并打印出插入的总数。

**注意**：
- 在使用此函数之前，需要确保VecDB数据库已经正确配置，并且`vecdb.insert_sentence`方法可以正常工作。
- `pop_tool_call_process`函数的具体实现和作用在此代码段中没有给出，需要查看相关代码以了解其详细行为。
- `workflow_insert_vec`函数同样没有在此代码段中定义，需要查看该函数的实现来了解它是如何插入子树的。
- 此函数中注释掉的部分代码可能是用于插入单步骤计划的，但在当前版本中未被使用。如果未来需要使用单步骤计划的插入功能，可能需要取消注释并实现`dict_insert_vec`函数。
- 函数中使用了`json.dumps`来将计划数据转换为JSON格式的字符串，这里指定了`indent=2`和`ensure_ascii=False`参数，以便生成更易读且支持非ASCII字符的JSON字符串。
***
