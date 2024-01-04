# ClassDef Credentials
**Credentials 类的功能**: 此类的功能是管理和查询n8n工作流引擎的凭证数据。

Credentials 类用于加载和处理n8n工作流引擎的凭证数据。它提供了方法来获取工作流ID以及查询特定节点类型的凭证信息。

当创建一个Credentials对象时，构造函数`__init__`会被调用。构造函数接受一个可选参数`base_file_path`，该参数指定了凭证文件的基础路径。如果没有提供这个参数，将使用默认路径`"./ProAgent/n8n_tester/credentials"`。

在构造函数中，首先会尝试打开路径为`base_file_path`下的`c.json`文件，并从中读取凭证数据。凭证数据被存储在一个字典`credential_data`中，其中每个条目包含了凭证的名称、ID和类型。接着，对于每个凭证条目，函数会检查它可以访问的节点类型，并将这些信息组织成一个以节点类型名称为键，凭证信息列表为值的字典`self.credential_data`。

此外，构造函数还会打开`base_file_path`下的`w.json`文件，从中读取工作流数据，并将第一个工作流的ID存储在`self.workflow_id`属性中。

`get_workflow_id`方法用于获取存储在对象中的工作流ID。它返回一个字符串类型的工作流ID。

`query`方法接受一个节点类型名称作为参数，并查询与该节点类型关联的凭证数据。如果给定的节点类型在`self.credential_data`字典中存在，它将返回该类型的最后一个凭证信息；如果不存在，则返回None。

**注意**:
- 在使用Credentials类时，确保`c.json`和`w.json`文件存在于指定的`base_file_path`路径下，并且它们的格式正确，以便类能够正确解析数据。
- 当查询不存在的节点类型时，`query`方法将返回None，调用者应该检查返回值以避免潜在的错误。

**输出示例**:
假设`c.json`文件中包含以下内容：
```json
[
    {
        "name": "MyCredential",
        "id": "1",
        "type": "apiKey",
        "nodesAccess": [
            {
                "nodeType": "com.example.MyNode"
            }
        ]
    }
]
```
并且`w.json`文件中包含以下内容：
```json
[
    {
        "id": "workflow_123"
    }
]
```
那么，创建Credentials对象并调用其方法的示例代码及其输出将如下所示：
```python
credentials = Credentials()
workflow_id = credentials.get_workflow_id()
print(workflow_id)  # 输出: workflow_123

credential_info = credentials.query("MyNode")
print(credential_info)  # 输出: {'name': 'MyCredential', 'id': '1', 'type': 'apiKey'}
```
## FunctionDef __init__
**__init__函数**: 该函数的功能是初始化CredentialLoader对象，并加载指定路径下的凭证数据和工作流ID。

该`__init__`方法是`CredentialLoader`类的构造函数，它负责初始化对象，并从文件中加载凭证数据和工作流ID。构造函数接受一个可选参数`base_file_path`，该参数指定了凭证文件的基本路径。如果调用时没有提供这个参数，将使用默认路径`"./ProAgent/n8n_tester/credentials"`。

在方法内部，首先通过打开路径为`base_file_path`下的`"c.json"`文件，读取JSON格式的凭证数据。然后，它创建一个空字典`self.credential_data`，用于存储解析后的凭证信息。对于`c.json`中的每一项凭证，方法会提取凭证的名称、ID和类型，并将这些信息存储为一个字典`item_info`。接着，对于每个凭证允许访问的节点类型，方法会提取节点类型名称，并将`item_info`添加到`self.credential_data`字典中对应节点类型名称的列表中。这样，`self.credential_data`字典就包含了按节点类型分类的凭证信息。

接下来，方法打开路径为`base_file_path`下的`"w.json"`文件，读取JSON格式的工作流数据。然后，它将第一个工作流的ID赋值给`self.workflow_id`属性。这样，`CredentialLoader`对象就包含了必要的凭证数据和工作流ID，以供后续使用。

**注意**:
- 使用`CredentialLoader`类时，需要确保`base_file_path`路径下存在`"c.json"`和`"w.json"`这两个文件，且文件格式符合预期的JSON结构。
- 在处理文件和JSON数据时，应当注意异常处理，例如文件不存在或JSON格式错误，但在这段代码中并未显示处理这些潜在的异常。
- `self.credential_data`字典的结构设计为按节点类型名称分类存储凭证信息，这样可以方便地根据节点类型查找相关的凭证。
- 该构造函数假设`"w.json"`文件中至少包含一个工作流对象，并且使用了列表中第一个工作流对象的ID。如果文件中没有工作流对象，或者结构不符合预期，可能会导致后续操作出现问题。
## FunctionDef get_workflow_id
**get_workflow_id函数**: 此函数的功能是获取工作流ID。

`get_workflow_id`函数定义在`n8n_tester`模块的`credential_loader.py`文件中，它的作用是返回一个字符串类型的工作流ID。这个ID是工作流的唯一标识符，用于在n8n工作流自动化平台中识别和管理工作流。

在`run_node.py`文件中，`get_workflow_id`函数被调用以获取当前工作流的ID。这个ID随后被用于设置`constant_workflow`字典中的`id`字段，这个字典代表了一个n8n工作流的配置。在执行n8n节点之前，需要确保工作流的ID是正确的，以便正确地执行工作流中的节点。

在`run_node`函数中，`get_workflow_id`的返回值被直接赋值给`constant_workflow["id"]`。这表明在执行n8n节点之前，系统需要知道工作流的ID，以便能够在正确的工作流上下文中执行节点。

**注意**:
- 使用`get_workflow_id`函数时，需要确保`self.workflow_id`已经被正确赋值，否则会返回一个无效的ID。
- 此函数假设`self`对象已经有一个名为`workflow_id`的属性，且该属性存储了有效的工作流ID。

**输出示例**:
假设当前工作流的ID为`"12345"`，那么调用`get_workflow_id`函数将返回字符串`"12345"`。
## FunctionDef query
**query函数**: 这个函数的功能是检索与给定节点类型相关联的凭据数据的最后一个元素。

该函数接受一个参数：
- node_type (str): 节点的类型。

返回值：
- 与给定节点类型相关联的凭据数据的最后一个元素，如果找不到节点类型则返回None。

该函数首先检查凭据数据中是否存在与给定节点类型相关联的数据。如果不存在，则返回None。如果存在，则返回凭据数据中与给定节点类型相关联的最后一个元素。

在项目中，该函数在以下文件中被调用：
文件路径：XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/run_node.py
调用代码如下：
def run_node(node: n8nPythonNode, input_data: list[dict] = [{}]) -> tuple[str, str]:
    """执行指定的节点。

    参数:
        workflow_id (Optional[str], optional): 节点所在工作流的ID。工作流ID必须存在于n8n工作流数据库中。您可以创建一个工作流并选择该ID。默认为None。
        node (Optional[dict], optional): n8n节点的JSON字典。如果未提供，则使用默认的Slack发送消息节点。默认为None。
        input_data (list[dict], optional): 节点的输入数据。默认为[{}]。

    返回值:
        tuple[str, str]: 包含两个字符串的元组。第一个字符串表示节点执行的状态（例如“成功”，“失败”），第二个字符串提供与执行相关的附加信息或错误消息。
    """
    # 问题：并行执行
    constant_workflow = _get_constant_workflow(input_data=input_data)

    constant_workflow["id"] = credentials.get_workflow_id()
    node_var = constant_workflow["nodes"][-1]
    node_var["type"] = "n8n-nodes-base." + node.node_meta.integration_name

    if credentials.query(node.node_meta.integration_name) != None:
        credential_item = credentials.query(node.node_meta.integration_name)
        node_var["credentials"] = {
            credential_item["type"]: {
                "id": credential_item["id"],
                "name": credential_item["name"],
            }
        }

    param_json = {}
    for key, value in node.params.items():
        param = value.to_json()
        if param != None:
            param_json[key] = param
    
    if 'json' in input_data[0].keys():
        node_var['parameters'] = input_data[0]['json']
        node_var["parameters"].update(param_json)
    else:
        node_var["parameters"] = param_json


    node_var["parameters"]["operation"] = node.node_meta.operation_name
    node_var["parameters"]["resource"] = node.node_meta.resource_name

    if node.node_meta.integration_name == 'slack':
        node_var["parameters"]["authentication"] = "oAuth2"
        
    if node.node_meta.integration_name == 'googleSheets':
        node_var["parameters"]["operation"] = node.node_meta.operation_name
        node_var["typeVersion"] = 4
        node_var["parameters"]["columns"] = {
                    "mappingMode": "autoMapInputData",
                    "value": {},
                    "matchingColumns": [
                      "id"
                    ]
                }


    # 处理工作流
    if 'pseudoNode' in node.node_json.keys() and node.node_json['pseudoNode']:
        try:
            import pdb; pdb.set_trace()
            output = run_pseudo_workflow(input_data, constant_workflow)
            error= ""
        except BaseException as e:
            traceback.print_exc()
            print(e)
            raise e
    else:
        temp_file = tempfile.NamedTemporaryFile(delete=False, mode="w", suffix=".json")
        json.dump(constant_workflow, temp_file)
        temp_file.close()
        temp_file_path = temp_file.name
        # import pdb; pdb.set_trace()
        result = subprocess.run(["n8n", "execute", "--file", temp_file_path], stdout=subprocess.PIPE, stderr=subprocess.PIPE) 
        # 获取标准输出
        output = result.stdout.decode('utf-8')
        error = result.stderr.decode('utf-8')

    print(colored("###OUTPUT###", color="green"))
    print(colored(output, color="green"))

    print(colored("###ERROR###", color="red"))
    print(colored(error, color="red"))

    output_data = ""
    error = ""

    # 检查输入数据
    if input_data == None or len(input_data) == 0:
        warning_prompt = "警告：input_data中没有任何内容。这可能导致当前节点执行失败。\n"
        print(colored(warning_prompt, color='yellow'))
        output_data += warning_prompt

    if success_prompt in output:
        output_data = output.split(success_prompt)[-1]
    else:
        assert error_prompt in output_data
        outputs = output.split(error_prompt)
        assert len(outputs) == 2
        output_data = outputs[0]
        error = outputs[1].strip()

    if output_data != "":
        output_data = json.loads(output_data)
        output_data = output_data["data"]["resultData"]["runData"]["node_var"][0]["data"]["main"][0]
    else:
        output_data = []

    return output_data, error
[代码片段结束]
[End of XAgent/engines/dsl/DSL_runner/ProAgent/n8n_tester/run_node.py]

请根据目标对象本身的代码以及其在项目中的调用情况，生成一个详细的解释文档。

请注意：
- 生成的内容中不应包含Markdown的层级标题和分隔符语法。
- 主要使用中文撰写。如果需要，可以在分析和描述中使用一些英文单词，以增强文档的可读性，因为您不需要将函数名或变量名翻译为目标语言。
***
