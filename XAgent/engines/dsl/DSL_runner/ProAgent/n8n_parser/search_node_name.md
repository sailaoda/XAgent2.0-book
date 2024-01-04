# FunctionDef search_node_name
**search_node_name函数**: 该函数的功能是在JSON文件中搜索指定名称的节点，并返回相应的信息。

该函数`search_node_name`接受两个参数：`file_path`和`search_name`。`file_path`参数指定了JSON文件的路径，默认值为`"./nodes.json"`。`search_name`参数指定了需要在JSON文件中搜索的节点名称，默认值为`"Spreadsheet File"`。

函数开始时，会打开指定路径的JSON文件，并使用`json.load`方法将文件内容加载为JSON对象。随后，函数遍历JSON对象中的每一个条目，对每个条目的`displayName`属性进行比较，以确定是否与`search_name`参数值相匹配。比较时，会将两个字符串都转换为小写，以确保比较不受大小写影响。

如果找到匹配的节点，函数会进一步搜索该节点的`properties`属性，以收集节点的资源（resource）和操作（operation）信息。资源和操作信息分别存储在`res_list`和`op_list`列表中。这些信息是通过遍历节点的`properties`属性，并检查每个属性的`displayName`是否为"Resource"或"Operation"来收集的。如果是，则将该属性的`options`中的所有`value`值添加到相应的列表中。

最后，如果找到了匹配的节点，函数会返回一个字典，包含节点的`name`、`resource`和`operation`信息。如果在JSON文件中没有找到匹配的节点，函数将返回`None`。

**注意**：
- 确保传入的`file_path`参数是有效的JSON文件路径，且文件格式正确，否则可能会引发异常。
- 传入的`search_name`应该是准确的节点显示名称，否则可能无法找到对应的节点。
- 函数返回的字典中的`resource`和`operation`是列表形式，可能包含多个值。

**输出示例**：
如果在nodes.json文件中存在一个名为"Spreadsheet File"的节点，其资源为["Google Sheets", "Excel"]，操作为["Read", "Write"]，那么函数的返回值可能如下所示：
```python
{
    "name": "SpreadsheetFile",
    "resource": ["Google Sheets", "Excel"],
    "operation": ["Read", "Write"]
}
```
如果没有找到匹配的节点，则函数返回`None`。
***
