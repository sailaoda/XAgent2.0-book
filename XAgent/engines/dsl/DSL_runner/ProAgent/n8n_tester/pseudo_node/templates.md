# FunctionDef format_replace_exp_workflow
**format_replace_exp_workflow函数**：该函数的功能是生成一个用于替换表达式的格式化值的工作流。

该函数接受一个数据项和一个表达式作为输入，并生成一个用于替换表达式的格式化值的工作流。data_item参数是一个包含要格式化的数据的字典。expression参数是一个表示要替换的表达式的字符串。

函数返回一个表示生成的替换表达式工作流的字典。该工作流用于执行工作流触发器，执行代码并将格式化值赋给一个变量。

以下是该函数的详细分析和描述：

- 参数：
  - data_item (dict)：要格式化的数据项。
  - expression (str)：要替换的表达式。

- 返回值：
  - dict：表示生成的替换表达式的工作流。

- 示例：
  ```python
  format_replace_exp_workflow(
      {
          "name": "John",
          "age": 30
      },
      "Hello, {name}! You are {age} years old."
  )
  ```

该函数接受一个数据项和一个表达式作为输入，并生成一个用于替换表达式的格式化值的工作流。data_item参数是一个包含要格式化的数据的字典。expression参数是一个表示要替换的表达式的字符串。

函数返回一个字典，表示生成的替换表达式的工作流。该工作流包含以下节点：

- Execute Workflow Trigger节点：用于执行工作流触发器。
- Code节点：用于执行代码，将data_item作为输入。
- node_var节点：用于执行代码，将expression替换为格式化值，并将结果赋给一个变量。

该函数的示例演示了如何使用该函数生成一个工作流，将字符串"Hello, {name}! You are {age} years old."中的表达式"{name}"替换为值"John"，将表达式"{age}"替换为值"30"。

**注意**：该函数依赖于其他函数和工具，如format_expression函数和n8n命令行工具。在使用该函数之前，请确保这些依赖项已正确安装和配置。

**输出示例**：
```python
{
    "meta": {
        "instanceId": "c14b6327b27c1dfe99ed7d452fafcd4d8e371aa8906a65b2508cc554e201c539"
    },
    "nodes": [
        {
            "id": "64074a51-e223-4f81-9343-12f8e6d0e536",
            "name": "Execute Workflow Trigger",
            "type": "n8n-nodes-base.executeWorkflowTrigger",
            "typeVersion": 1,
            "position": [
                0,
                0
            ],
            "parameters": {}
        },
        {
            "parameters": {
                "jsCode": "return [{\"name\": \"John\", \"age\": 30}]"
            },
            "id": "5b7fc8cc-50d0-48d4-bdf1-7e55d0b29521",
            "name": "Code",
            "type": "n8n-nodes-base.code",
            "typeVersion": 2,
            "position": [
                -60,
                820
            ]
        },
        {
            "parameters": {
                "mode": "runOnceForEachItem",
                "jsCode": "$input.item.json = {\n  formatted:`Hello, John! You are 30 years old.`\n}\n\nreturn $input.item;"
            },
            "id": "2326a464-286f-46d4-8059-1b510bbd1654",
            "name": "node_var",
            "type": "n8n-nodes-base.code",
            "typeVersion": 2,
            "position": [
                120,
                800
            ]
        }
    ],
    "connections": {
        "Execute Workflow Trigger": {
            "main": [
                [
                    {
                        "node": "Code",
                        "type": "main",
                        "index": 0
                    }
                ]
            ]
        },
        "Code": {
            "main": [
                [
                    {
                        "node": "node_var",
                        "type": "main",
                        "index": 0
                    }
                ]
            ]
        }
    },
    "active": false,
    "settings": {
        "executionOrder": "v1"
    },
    "tags": [],
    "id": "ovm4ntEU37IYs7PI"
}
```
***
# FunctionDef format_return_data
**format_return_data 函数**: 此函数的功能是生成给定返回列表的格式化返回数据。

此函数接受一个列表作为参数，该列表包含需要被格式化的数据。它返回一个字符串，该字符串是一个预定义模板的实例，其中包含了执行成功的信息和相关的执行数据。

函数内部定义了一个多行字符串 `return_data_template`，这个模板包含了一系列的执行结果数据，例如执行开始时间、执行时间、执行状态以及其他相关信息。模板中的 `{return_list}` 占位符用于插入传入函数的 `return_list` 参数。

函数通过 `json.dumps(return_list)` 将传入的列表转换为 JSON 格式的字符串，然后使用字符串的 `replace` 方法将模板中的 `{return_list}` 占位符替换为转换后的 JSON 字符串。最终，函数返回这个替换后的模板字符串。

在项目中，`format_return_data` 函数被 `utils.py` 文件中的 `fill_return_data` 函数调用。`fill_return_data` 函数接收一个输出数据列表，并调用 `format_return_data` 函数来生成格式化的返回数据，然后返回这个数据。

**注意**：
- 确保传递给 `format_return_data` 函数的 `return_list` 参数是一个列表，因为函数内部会将其转换为 JSON 字符串。
- 模板字符串中的其他字段（如执行时间、状态等）已经预设在模板中，调用者需要确保这些预设值符合实际情况或根据需要进行相应的调整。

**输出示例**：
```json
Execution was successful:
====================================
{
    "data": {
        "startData": {},
        "resultData": {
            "runData": {
                "Execute Workflow Trigger": [
                    {
                        "startTime": 1697883465922,
                        "executionTime": 0,
                        "source": [],
                        "executionStatus": "success",
                        "data": {
                            "main": [
                                [
                                    {
                                        "json": {},
                                        "pairedItem": {
                                            "item": 0
                                        }
                                    }
                                ]
                            ]
                        }
                    }
                ],
                "Code": [
                    {
                        "startTime": 1697883465923,
                        "executionTime": 11,
                        "source": [
                            {
                                "previousNode": "Execute Workflow Trigger"
                            }
                        ],
                        "executionStatus": "success",
                        "data": {
                            "main": [
                                [
                                    {
                                        "json": {},
                                        "pairedItem": {
                                            "item": 0
                                        }
                                    }
                                ]
                            ]
                        }
                    }
                ],
                "node_var": [
                    {
                        "startTime": 1697883465934,
                        "executionTime": 48,
                        "source": [
                            {
                                "previousNode": "Code"
                            }
                        ],
                        "executionStatus": "success",
                        "data": {
                            "main": [
                                [
                                    {
                                        "json": {"exampleKey": "exampleValue"},
                                        "pairedItem": {
                                            "item": 0
                                        }
                                    }
                                ]
                            ]
                        }
                    }
                ]
            },
            "lastNodeExecuted": "node_var"
        },
        "executionData": {
            "contextData": {},
            "nodeExecutionStack": [],
            "waitingExecution": {},
            "waitingExecutionSource": {}
        }
    },
    "mode": "cli",
    "startedAt": "2023-10-21T10:17:45.909Z",
    "stoppedAt": "2023-10-21T10:17:45.982Z",
    "status": "running",
    "finished": true
}
```
在上述示例中，`{return_list}` 被替换为了一个包含示例键值对的 JSON 对象。
***
