# ClassDef n8nParameterType
**n8nParameterType类**：该类定义了n8n json中支持的参数类型。

该类具有以下属性：
- STRING：字符串类型
- NUMBER：数字类型
- BOOLEAN：布尔类型
- LIST：列表类型
- JSON：JSON类型
- COLOR：颜色类型
- DATATIME：日期时间类型
- COLLECTION：集合类型
- FIXEDCOLLECTION：固定集合类型
- OPTIONS：选项类型
- MULTIOPTIONS：多选项类型
- RESOURCELOCATOR：资源定位器类型
- RESOURCEMAPPER：资源映射器类型
- NOTICE：通知类型

该类被以下文件调用：
- XAgent/engines/dsl/DSL_runner/ProAgent/n8n_parser/parameters.py

**visit_parameter函数**：根据参数的类型访问参数并返回结果。

参数：
- param_json (dict)：以JSON格式表示的参数字典。

返回值：
- 根据参数类型访问参数并返回结果。

**n8nParameter类**：该类是n8n参数的基类，定义了参数的基本属性和方法。

该类具有以下属性：
- father：父节点
- param_type：参数类型
- name：参数名称
- required：是否必需
- default：默认值
- description：参数描述
- no_data_expression：是否允许使用表达式
- display_string：显示字符串
- multiple_values：是否允许多个值
- use_expression：是否使用表达式
- data_is_set：数据是否已设置

该类具有以下方法：
- \_\_init\_\_：初始化类实例
- get_depth：获取当前节点在树中的深度
- visit：访问参数并返回节点
- to_description：生成参数的描述
- parse_value：解析给定的值
- get_parameter_name：获取参数在n8n工作流中的名称
- refresh：刷新数据

**n8nFixedCollection类**：该类继承自n8nParameter类，表示固定集合类型的参数。

该类具有以下属性：
- meta：元数据
- value：值

该类具有以下方法：
- \_\_init\_\_：初始化类实例
- visit：访问参数并返回节点
- to_json：将对象转换为JSON表示
- parse_value：解析给定的值
- refresh：刷新数据
- to_description：生成参数的描述

**注意**：在使用n8nParameterType类时，需要注意参数的类型和属性，以及参数在n8n工作流中的使用方式。
***
# FunctionDef visit_parameter
**visit_parameter函数**: 这个函数的作用是根据参数的类型访问参数并返回结果。

该函数接受一个以JSON格式表示的参数字典作为输入。

返回值为根据参数类型访问参数后的结果。

在该函数中，首先获取参数的类型。然后根据参数类型调用相应的visit函数进行访问，并返回访问结果。

如果参数类型为BOOLEAN，则调用n8nBoolean.visit函数进行访问。

如果参数类型为NUMBER，则调用n8nNumber.visit函数进行访问。

如果参数类型为STRING，则调用n8nString.visit函数进行访问。

如果参数类型为OPTIONS，则调用n8nOption.visit函数进行访问。

如果参数类型为COLLECTION，则调用n8nCollection.visit函数进行访问。

如果参数类型为FIXEDCOLLECTION，则调用n8nFixedCollection.visit函数进行访问。

如果参数类型为RESOURCELOCATOR，则调用n8nResourceLocator.visit函数进行访问。

如果参数类型不在以上类型中，则打印参数名称和参数类型，表示未解析。

在该函数中，通过判断参数类型来选择不同的visit函数进行访问，并返回访问结果。

**注意**: visit_parameter函数的输入参数param_json必须是一个以JSON格式表示的参数字典。

**输出示例**:
如果参数类型为BOOLEAN，返回n8nBoolean.visit(param_json)的结果。

如果参数类型为NUMBER，返回n8nNumber.visit(param_json)的结果。

如果参数类型为STRING，返回n8nString.visit(param_json)的结果。

如果参数类型为OPTIONS，返回n8nOption.visit(param_json)的结果。

如果参数类型为COLLECTION，返回n8nCollection.visit(param_json)的结果。

如果参数类型为FIXEDCOLLECTION，返回n8nFixedCollection.visit(param_json)的结果。

如果参数类型为RESOURCELOCATOR，返回n8nResourceLocator.visit(param_json)的结果。

如果参数类型不在以上类型中，则打印参数名称和参数类型。
***
# ClassDef n8nParameter
**n8nParameter类功能**：n8nParameter类是一个参数类，用于表示n8n工作流中的参数。它包含了参数的各种属性和方法，用于解析和处理参数的值。

该类具有以下属性：
- father: 表示父级参数对象，默认为None。
- param_type: 表示参数的类型，默认为n8nParameterType.ERROR。
- name: 表示参数的名称，默认为空字符串。
- required: 表示参数是否为必需，默认为False。
- default: 表示参数的默认值，默认为None。
- description: 表示参数的描述，默认为空字符串。
- no_data_expression: 表示参数是否支持表达式，默认为False。
- display_string: 表示参数的显示字符串，默认为空字符串。
- multiple_values: 表示参数是否允许多个值，默认为False。
- use_expression: 表示参数是否使用表达式，默认为False。
- data_is_set: 表示参数的数据是否已设置，默认为False。

该类具有以下方法：
- \_\_init\_\_(self, param_json): 初始化类的实例。
- get_depth(self): 返回当前节点在树中的深度。
- visit(cls, param_json): 静态方法，根据给定的param_json创建一个n8nParameter节点。
- to_description(self, prefix_ids, indent=2, max_depth=1): 将参数对象转换为描述字符串。
- parse_value(self, value: any) -> (ToolCallStatus, str): 解析参数的值并返回解析结果。
- get_parameter_name(self): 返回当前节点在n8n工作流中的参数名称。
- refresh(self): 刷新数据，将data_is_set属性设置为False。
- to_json(self): 将对象转换为JSON格式。

**注意**：
- n8nParameter类是一个抽象类，不能直接实例化。
- 该类的具体子类根据不同的参数类型进行实现，包括n8nNotice、n8nNumber、n8nBoolean、n8nString、n8nOption、n8nCollection和n8nFixedCollection。

**输出示例**：
```python
class n8nParameter():
    father: 'n8nParameter' = None
    param_type: n8nParameterType = n8nParameterType.ERROR 

    name: str = ""
    required: bool = False
    default: Any = None
    description: str = ""
    no_data_expression: bool = False
    display_string: str = ""
    multiple_values: bool = False

    use_expression: bool = False
    data_is_set: bool = False

    def __init__(self, param_json):
        """
        初始化类的实例。
        
        Args:
            param_json (dict): 包含函数参数的字典。
        
        Returns:
            None
        """
        if "type" in param_json.keys():
            self.param_type = n8nParameterType(param_json["type"])
        if "name" in param_json.keys():
            self.name = param_json["name"]
        if "default" in param_json.keys():
            self.default = param_json["default"]
        if "required" in param_json.keys():
            self.required = param_json["required"]
        if "displayName" in param_json.keys():
            self.description = param_json["displayName"]
        if "description" in param_json.keys():
            self.description +=  "。" + param_json["description"]
        if "placeholder" in param_json.keys():
            self.description +=  f"（{param_json['placeholder']}）"

        if "noDataExpression" in param_json.keys():
            self.no_data_expression = param_json["noDataExpression"]


        if "displayOptions" in param_json.keys():
            if "show" in param_json.keys():
                for instance in param_json["displayOptions"]["show"]:
                    if instance not in ["resource", "operation"]:
                        expression = f"{instance} in {param_json['displayOptions']['show'][instance]}"
                        if self.display_string != "":
                            self.display_string += " and "
                        self.display_string += expression

        if "typeOptions" in param_json.keys() and "multipleValues" in param_json["typeOptions"].keys():
            self.multiple_values = param_json["typeOptions"]["multipleValues"]

    def get_depth(self):
        """
        返回当前节点在树中的深度。

        Returns:
            int: 当前节点在树中的深度。
        """
        if self.father == None:
            return 1
        return self.father.get_depth() + 1

    @classmethod
    @abstractmethod
    def visit(cls, param_json):
        pass
    
    @abstractmethod
    def to_description(self, prefix_ids, indent=2, max_depth=1):
        pass

    @abstractmethod
    def parse_value(self, value: any) -> (ToolCallStatus, str):
        return ToolCallStatus.ToolCallSuccess, f"{self.get_parameter_name()}未实现"

    def get_parameter_name(self):
        """
        返回n8n工作流中当前节点的参数名称。

        Returns:
            str: 当前节点的参数名称。

        Raises:
            AssertionError: 如果参数类型为`FIXEDCOLLECTION`且父节点有多个值。
        """

        if self.father == None:
            return f"params[\"{self.name}\"]"

        prefix_names = self.father.get_parameter_name()

        if self.father.param_type == n8nParameterType.COLLECTION and self.father.multiple_values:
            prefix_names += "[0]"
            return prefix_names +f"[\"{self.name}\"]"
        elif self.father.param_type == n8nParameterType.FIXEDCOLLECTION and self.father.multiple_values:
            assert self.param_type == n8nParameterType.COLLECTION, f"{self.param_type.name}"
            names = f"{prefix_names}[\"{self.name}\"]"
            return names
        elif self.father.param_type == n8nParameterType.RESOURCELOCATOR:
            for key,value in self.father.meta.items():
                if value == self:
                    name = f"{prefix_names}[\"value\"](when \"mode\"=\"{key}\")"
                    return name
        else:
            return prefix_names +f"[\"{self.name}\"]"
    
    def refresh(self):
        """
        刷新数据，将`data_is_set`属性设置为False。
        """
        self.data_is_set = False
```
## FunctionDef get_depth
**get_depth函数**：该函数用于返回当前节点在树中的深度。

该函数的具体代码如下：
```python
def get_depth(self):
    """
    Returns the depth of the current node in the tree.

    Returns:
        int: The depth of the current node in the tree.
    """
    if self.father == None:
        return 1
    return self.father.get_depth() + 1
```

**注意**：该函数是一个递归函数，用于计算当前节点在树中的深度。它通过调用父节点的`get_depth`函数，并将结果加1来计算深度。如果当前节点没有父节点，则深度为1。

**输出示例**：假设当前节点的深度为3，则函数返回值为3。
## FunctionDef get_parameter_name
**get_parameter_name函数**：该函数的作用是获取n8n工作流中当前节点的参数名称。

该函数首先判断当前节点是否有父节点，如果没有父节点，则直接返回参数名称。如果有父节点，则通过递归调用父节点的get_parameter_name函数获取父节点的参数名称。

接下来根据父节点的参数类型和是否允许多个值来确定参数名称的前缀。如果父节点的参数类型是COLLECTION并且允许多个值，则在前缀名称后添加"[0]"，然后再添加当前节点的名称。如果父节点的参数类型是FIXEDCOLLECTION并且允许多个值，则断言当前节点的参数类型也是COLLECTION，并将前缀名称和当前节点的名称拼接起来作为参数名称。如果父节点的参数类型是RESOURCELOCATOR，则遍历父节点的meta属性，找到与当前节点相等的值，并根据相应的key生成参数名称。如果以上条件都不满足，则将前缀名称和当前节点的名称拼接起来作为参数名称。

最后返回参数名称。

**注意**：如果父节点的参数类型是FIXEDCOLLECTION并且允许多个值，当前节点的参数类型必须是COLLECTION，否则会抛出AssertionError异常。

**输出示例**：假设当前节点的名称为"param1"，父节点的参数类型为COLLECTION并且允许多个值，则返回的参数名称为"params[0]["param1"]"。
***
# ClassDef n8nNotice
**n8nNotice 类的功能**：这个类的功能是创建和管理 n8n 工作流中的通知参数。

n8nNotice 类继承自 n8nParameter 类，用于表示 n8n 工作流节点中的通知参数。这个类包含一个字符串类型的属性 `notice`，用于存储通知的内容。

- `__init__(self, param_json)` 方法：这是 n8nNotice 类的初始化方法。它接收一个字典 `param_json` 作为参数，该字典包含了参数的相关信息。这个方法调用了父类 n8nParameter 的初始化方法来完成基础的设置。

- `visit(param_json)` 静态方法：这个方法接收一个字典 `param_json`，该字典包含了参数的相关信息。方法内部会创建一个 n8nNotice 实例，并将 `param_json` 中的 "displayName" 字段的值赋给实例的 `notice` 属性。最后返回这个创建好的 n8nNotice 实例。

- `to_description(self, prefix_ids, indent=2, max_depth=1)` 方法：这个方法用于生成参数的描述信息。当前的实现中，这个方法返回一个空列表，这可能意味着通知参数不需要额外的描述信息，或者描述信息的生成尚未实现。

**注意**：
- 在使用 n8nNotice 类时，需要确保传入的 `param_json` 字典中包含 "displayName" 键，否则在访问 `param_json["displayName"]` 时可能会抛出 KeyError 异常。
- `to_description` 方法目前返回一个空列表，如果需要生成描述信息，可能需要对这个方法进行进一步的实现。

**输出示例**：
由于 `to_description` 方法返回空列表，因此没有具体的输出示例。但是，如果我们假设这个方法被扩展以生成描述信息，那么它可能会返回类似以下内容的列表：

```python
[
    "这是一个通知参数，用于在工作流执行中提供重要信息。",
    "通知内容：用户成功创建了一个新的工作流。"
]
```

这只是一个假设的示例，实际的输出将取决于方法的具体实现和 `param_json` 中的数据。
***
# ClassDef n8nNumber
**n8nNumber函数**: 这个类的功能是表示n8n工作流中的数字参数。

n8nNumber类是n8nParameter类的子类，它用于表示n8n工作流中的数字参数。该类具有以下属性：

- fixed_value: 表示固定的数值，默认为0.0。
- var: 表示表达式的变量，默认为空字符串。

该类具有以下方法：

- \_\_init\_\_(self, param_json): 类的构造函数，用于初始化对象的属性。
- visit(cls, param_json): 类方法，用于根据传入的参数类型创建n8nNumber对象。
- parse_value(self, value: any) -> (ToolCallStatus, str): 抽象方法，用于解析传入的数值，并返回解析结果和解析信息。
- to_description(self, prefix_ids, indent=2, max_depth=1): 将参数转化为描述信息的方法，返回描述信息的列表。
- to_json(self): 将参数转化为JSON格式的方法，返回转化后的JSON对象。

**注意**: 
- n8nNumber类继承自n8nParameter类，因此它继承了n8nParameter类的属性和方法。
- parse_value方法根据传入的数值类型进行解析，如果数值为整数或布尔值，则将其解析为固定的数值；如果数值为字符串，则将其解析为表达式的变量。
- to_description方法将参数转化为描述信息的字符串，包括参数名称、类型、默认值、是否必需、描述等信息。
- to_json方法将参数转化为JSON格式的对象，如果参数使用了表达式，则返回表达式的变量；否则返回固定的数值。

**输出示例**:
```
n8nNumber: NUMBER = 0.0, Required: 数字参数，表示n8n工作流中的数字值。你不能使用表达式。
```
***
# ClassDef n8nBoolean
**n8nBoolean函数**: 这个类的功能是表示n8n工作流中的布尔类型参数。

该类继承自n8nParameter类，用于表示n8n工作流中的布尔类型参数。布尔类型参数可以是固定值或表达式。该类提供了解析参数值、生成描述行和转换为JSON格式的功能。

**parse_value函数**:
该函数用于解析给定的参数值，并返回解析后的值以及解析状态。如果参数值是布尔类型，则将其解析为固定值；如果参数值是字符串类型，则将其解析为表达式。如果参数值不是布尔类型或字符串类型，则返回参数类型错误。

**to_description函数**:
该函数将参数对象转换为描述行。描述行包括参数的名称、类型、默认值、是否必需、描述以及是否支持表达式。

**to_json函数**:
该函数将参数对象转换为JSON格式。如果参数对象的数据未设置，则返回None；如果参数对象使用表达式，则返回表达式字符串；如果参数对象使用固定值，则返回布尔值。

**visit函数**:
该函数是一个类方法，用于创建n8nBoolean对象并返回。该函数接收一个表示参数的JSON格式的字典，并将其传递给n8nBoolean的构造函数。

**注意**:
- n8nBoolean类是n8nParameter的子类，用于表示n8n工作流中的布尔类型参数。
- 该类提供了解析参数值、生成描述行和转换为JSON格式的功能。
- 参数值可以是布尔类型或字符串类型。
- 如果参数值是布尔类型，则将其解析为固定值。
- 如果参数值是字符串类型，则将其解析为表达式。
- 如果参数值不是布尔类型或字符串类型，则会返回参数类型错误。
- 参数对象可以包含默认值、是否必需、描述以及是否支持表达式等属性。
- 参数对象可以转换为描述行，描述行包括参数的名称、类型、默认值、是否必需、描述以及是否支持表达式。
- 参数对象可以转换为JSON格式，JSON格式可以用于序列化和传输参数对象。

**输出示例**:
```
n8nBoolean对象示例:
{
  "name": "is_active",
  "type": "boolean",
  "default": false,
  "required": true,
  "description": "是否激活",
  "no_data_expression": false
}
```
***
# ClassDef n8nString
**n8nString功能**：该类的功能是表示n8n工作流中的字符串类型参数。

该类继承自n8nParameter类，具有以下属性和方法：

- value: str类型，表示参数的值，默认为空字符串。
- \_\_init\_\_(self, param_json): 构造函数，用于初始化参数对象。
- visit(cls, param_json): 类方法，根据参数的类型访问参数并返回结果。
- parse_value(self, value: any) -> (ToolCallStatus, str): 解析给定的值并返回解析状态和消息的抽象方法。
- to_description(self, prefix_ids, indent=2, max_depth=1): 将当前参数转换为描述字符串的方法。
- to_json(self): 将参数的值转换为JSON格式的方法。

**注意**：该类是n8n工作流中的字符串类型参数表示类，用于解析和描述字符串类型的参数。

**输出示例**：
```python
{
    "value": "example string"
}
```
***
# ClassDef n8nOption
**n8nOption函数**: 这个类的函数是用来表示n8n工作流中的选项参数的。

该类继承自n8nParameter类，表示n8n工作流中的选项参数。选项参数是一种特殊类型的参数，它具有一组预定义的值，用户只能从这些值中选择一个作为参数的取值。该类具有以下属性和方法：

- value: str类型，表示参数的值，默认为空字符串。
- enum: list类型，表示参数的可选值列表，默认为空列表。
- enum_descriptions: list类型，表示参数可选值的描述列表，默认为空列表。

该类的构造函数__init__接受一个参数param_json，该参数是一个字典，表示参数的JSON表示形式。构造函数首先调用父类的构造函数，然后初始化enum和enum_descriptions属性为空列表。

visit方法是一个类方法，用于从提供的JSON表示形式生成一个n8nOption对象。该方法接受一个参数param_json，表示参数的JSON表示形式。方法首先创建一个n8nOption对象node，然后根据param_json中的options字段，将enum和enum_descriptions属性填充为可选值的列表和描述列表。最后返回生成的node对象。

parse_value方法用于解析参数的值并返回相应的ToolCallStatus和字符串消息。该方法接受一个参数value，表示要解析的值。方法首先判断value的类型，如果是字符串类型，则根据不同的情况进行解析。如果value以双引号开头和结尾，则去除双引号。如果value中包含逗号，则返回ToolCallStatus.ParamTypeError和相应的错误消息。如果value以等号开头，则判断是否支持表达式，如果不支持则返回ToolCallStatus.UnsupportedExpression和相应的错误消息。否则，将value赋值给value属性，并将data_is_set属性设置为True，然后返回ToolCallStatus.ToolCallSuccess和相应的成功消息。如果value不是字符串类型，则返回ToolCallStatus.ParamTypeError和相应的错误消息。

to_description方法用于生成参数的描述信息。该方法接受三个参数：prefix_ids表示前缀ID，indent表示缩进的空格数，默认为2，max_depth表示描述的最大深度，默认为1。方法首先根据参数的属性生成第一行描述信息，包括参数的名称、类型、默认值、是否必需等信息。然后根据enum和enum_descriptions属性生成可选值的描述信息。最后返回描述信息的列表。

to_json方法用于返回对象的JSON表示形式。如果data_is_set属性为False，则返回None；否则返回value属性的值。

**注意**：
- n8nOption类是n8n工作流中选项参数的表示，用于表示具有预定义可选值的参数。
- 该类的构造函数接受一个参数param_json，该参数是参数的JSON表示形式。
- visit方法是一个类方法，用于从JSON表示形式生成n8nOption对象。
- parse_value方法用于解析参数的值并返回相应的状态和消息。
- to_description方法用于生成参数的描述信息。
- to_json方法用于返回对象的JSON表示形式。

**输出示例**：
```
n8nOption: enum[string] = ""
  参数描述: 这是一个选项参数。可选值:
    0 value=="option1": 选项1
    1 value=="option2": 选项2
    2 value=="option3": 选项3
```

***
# ClassDef n8nCollection
**n8nCollection函数**: 这个类的功能是表示一个n8n集合参数。

该类继承自n8nParameter类，用于表示n8n工作流中的集合类型参数。集合参数是一个包含多个键值对的字典或字典列表。该类具有以下属性和方法：

- 属性：
  - meta：一个字典，用于存储集合参数的元数据，其中键是参数的名称，值是参数的n8nParameter对象。
  - value：一个列表，用于存储集合参数的值，其中每个元素都是一个字典，表示一个键值对。

- 方法：
  - `__init__(self, param_json)`：初始化n8nCollection对象。接受一个包含参数初始化信息的字典作为参数。
  - `visit(cls, param_json)`：从给定的JSON表示生成n8nCollection对象。接受一个包含参数信息的字典作为参数，并返回生成的n8nCollection对象。
  - `parse_single_dict(self, value, list_count=False)`：解析单个字典值。接受一个要解析的字典值和一个布尔值（表示字典值是否属于列表）作为参数，并返回一个包含工具调用状态、消息和新字典的元组。
  - `parse_value(self, value: any)`：解析给定的值并返回工具调用状态码和结果字符串。接受一个要解析的值作为参数，并返回一个包含工具调用状态码和结果字符串的元组。
  - `to_description(self, prefix_ids, indent=2, max_depth=1)`：生成函数的描述，包括参数和返回类型。接受前缀ID、缩进和最大深度作为参数，并返回函数描述的行列表。
  - `refresh(self)`：刷新对象中的数据。将`data_is_set`设置为`False`，清空`value`，并调用`meta`中每个键对应的值的`refresh`方法。
  - `to_json(self)`：将对象转换为JSON表示。如果`data_is_set`为`False`，返回`None`。如果`multiple_values`为`True`，返回一个包含`value`属性中每个字典的列表。如果`multiple_values`为`False`，返回`value`属性中第一个字典的键值对表示。

**注意**：
- 该类是n8nParameter的子类，用于表示n8n工作流中的集合类型参数。
- `visit`方法用于从给定的JSON表示生成n8nCollection对象，并根据参数的类型生成相应的n8nParameter对象。
- `parse_single_dict`方法用于解析单个字典值，并根据参数的元数据进行解析。
- `parse_value`方法用于解析给定的值，并根据参数的类型和元数据进行解析。
- `to_description`方法用于生成函数的描述，包括参数和返回类型。
- `refresh`方法用于刷新对象中的数据，将`data_is_set`设置为`False`，清空`value`，并调用`meta`中每个键对应的值的`refresh`方法。
- `to_json`方法用于将对象转换为JSON表示。

**输出示例**：
```python
param = n8nCollection(param_json)
param.parse_value({"key1": "value1", "key2": "value2"})
print(param.to_json())
# 输出：{"key1": {"key": "value1"}, "key2": {"key": "value2"}}
```
## FunctionDef parse_single_dict
**parse_single_dict函数**: 这个函数的功能是解析单个字典值。

该函数接受一个字典值作为参数，并解析它。如果字典值不符合预期的格式，函数将返回一个包含工具调用状态、消息和一个新字典的元组。

函数首先会检查传入的值是否为字典类型，如果不是，则会抛出一个AssertionError异常。然后，函数会遍历字典的每个键，并检查键是否存在于元数据中。如果键不存在于元数据中，则会根据是否在列表中进行不同的处理。如果在列表中，则会生成一个带有列表索引的参数名称，否则生成普通的参数名称。然后，函数会返回一个表示未定义参数的工具调用状态、消息和一个空字典。

如果键存在于元数据中，函数会对该键对应的值进行深拷贝，并调用子参数的parse_value方法进行解析。如果子参数的解析状态不是ToolCallSuccess，则函数会返回子参数的解析状态、解析结果和一个空字典。否则，函数会将解析结果添加到新字典中。

最后，函数会返回ToolCallSuccess状态、空消息和新字典作为结果。

**注意**: 
- 如果传入的值不是字典类型，函数会抛出一个AssertionError异常。
- 函数的返回值是一个包含工具调用状态、消息和新字典的元组。

**输出示例**:
```
(ToolCallStatus.ToolCallSuccess, "", {"key": "value"})
```
***
# ClassDef n8nFixedCollection
**n8nFixedCollection类的功能**: n8nFixedCollection类是n8nParameter类的子类，用于表示固定集合类型的参数。它包含了元数据（meta）和值（value）两个属性，以及一些方法用于初始化、解析值、刷新数据和生成描述。

**__init__方法**:
- 初始化类实例，接受一个param_json参数作为初始化参数的字典。
- 参数:
  - param_json (dict): 包含初始化参数的字典。
- 返回值: 无

**visit方法**:
- 访问给定的param_json并返回一个n8nFixedCollection节点。
- 参数:
  - param_json (dict): 要访问的JSON参数。
- 返回值: 访问的n8nFixedCollection节点。
- 异常:
  - AssertionError: 如果param_json中不存在'options'键。

**to_json方法**:
- 将对象转换为JSON表示。
- 返回值: 包含对象JSON表示的字典。如果对象数据未设置，则返回None。

**parse_value方法**:
- 解析给定的值并返回一个包含工具调用状态和消息的元组。
- 参数:
  - value (any): 要解析的值。
- 返回值: 包含工具调用状态和消息的元组。
- 异常: 无
- 示例:
  - parse_value({"key": "value"}) -> (ToolCallStatus.ToolCallSuccess, "parameter parsed with keys: ['key']")

**refresh方法**:
- 刷新对象中的数据。
- 该函数将`data_is_set`属性设置为`False`，清空`value`属性，并通过调用每个对应值的`refresh`方法刷新`meta`字典中的每个键。
- 参数: 无
- 返回值: 无

**to_description方法**:
- 以正确的语言语法在markdown代码块中生成给定函数体的函数注释。
- 参数:
  - prefix_ids (str): 前缀ID。
  - indent (int, optional): 缩进的空格数。默认为2。
  - max_depth (int, optional): 最大深度。默认为1。
- 返回值: 函数注释的行列表。

**注意**: 
- n8nFixedCollection类继承自n8nParameter类，用于表示固定集合类型的参数。
- 该类具有初始化方法、访问方法、转换为JSON方法、解析值方法、刷新数据方法和生成描述方法。
- 通过调用visit方法可以访问给定的param_json并返回一个n8nFixedCollection节点。
- 该类的to_json方法将对象转换为JSON表示。
- parse_value方法用于解析给定的值，并返回一个包含工具调用状态和消息的元组。
- refresh方法用于刷新对象中的数据。
- to_description方法用于生成函数注释的行列表。

**输出示例**:
```python
n8nFixedCollection类的功能: n8nFixedCollection类是n8nParameter类的子类，用于表示固定集合类型的参数。它包含了元数据（meta）和值（value）两个属性，以及一些方法用于初始化、解析值、刷新数据和生成描述。

__init__方法:
- 初始化类实例，接受一个param_json参数作为初始化参数的字典。
- 参数:
  - param_json (dict): 包含初始化参数的字典。
- 返回值: 无

visit方法:
- 访问给定的param_json并返回一个n8nFixedCollection节点。
- 参数:
  - param_json (dict): 要访问的JSON参数。
- 返回值: 访问的n8nFixedCollection节点。
- 异常:
  - AssertionError: 如果param_json中不存在'options'键。

to_json方法:
- 将对象转换为JSON表示。
- 返回值: 包含对象JSON表示的字典。如果对象数据未设置，则返回None。

parse_value方法:
- 解析给定的值并返回一个包含工具调用状态和消息的元组。
- 参数:
  - value (any): 要解析的值。
- 返回值: 包含工具调用状态和消息的元组。
- 异常: 无
- 示例:
  - parse_value({"key": "value"}) -> (ToolCallStatus.ToolCallSuccess, "parameter parsed with keys: ['key']")

refresh方法:
- 刷新对象中的数据。
- 该函数将`data_is_set`属性设置为`False`，清空`value`属性，并通过调用每个对应值的`refresh`方法刷新`meta`字典中的每个键。
- 参数: 无
- 返回值: 无

to_description方法:
- 以正确的语言语法在markdown代码块中生成给定函数体的函数注释。
- 参数:
  - prefix_ids (str): 前缀ID。
  - indent (int, optional): 缩进的空格数。默认为2。
  - max_depth (int, optional): 最大深度。默认为1。
- 返回值: 函数注释的行列表。

注意: 
- n8nFixedCollection类继承自n8nParameter类，用于表示固定集合类型的参数。
- 该类具有初始化方法、访问方法、转换为JSON方法、解析值方法、刷新数据方法和生成描述方法。
- 通过调用visit方法可以访问给定的param_json并返回一个n8nFixedCollection节点。
- 该类的to_json方法将对象转换为JSON表示。
- parse_value方法用于解析给定的值，并返回一个包含工具调用状态和消息的元组。
- refresh方法用于刷新对象中的数据。
- to_description方法用于生成函数注释的行列表。
```
***
# ClassDef n8nResourceLocator
**n8nResourceLocator功能**：该类的功能是表示n8n资源定位器参数。

该类继承自n8nParameter类，并具有以下属性：
- meta: 一个字典，用于存储参数的元数据，默认为空字典。
- mode: 表示参数的模式，类型为字符串，默认为空字符串。
- value: 表示参数的值，类型为任意类型，默认为None。

该类具有以下方法：
- \_\_init\_\_(self, param_json): 初始化类并设置属性值。
- visit(cls, param_json): 访问参数JSON并创建一个节点对象。
- refresh(self): 刷新参数的数据状态。
- to_description(self, prefix_ids, indent=2, max_depth=1): 生成函数注释的描述。
- to_json(self): 将对象转换为JSON表示。
- parse_value(self, value: any) -> (ToolCallStatus, str): 解析给定的值并返回工具调用状态和消息。

**注意**：该类的构造函数需要传入一个param_json参数，该参数是一个包含初始化参数的字典。visit方法需要传入一个param_json参数，该参数是一个表示参数的JSON对象。to_description方法需要传入prefix_ids、indent和max_depth参数，其中prefix_ids是前缀ID，indent是缩进级别，max_depth是最大深度。parse_value方法需要传入一个value参数，该参数是要解析的值。

**输出示例**：
```
n8nResourceLocator: dict{"mode":enum(str),"values":any} = None, Required when (display_string), otherwise do not provide: 描述信息。 "mode" should be one of ['mode1', 'mode2']: 
  n8nString: str = None, Required when (display_string), otherwise do not provide: 描述信息。 You can't use expression.
  ...hidden...
```

以上是对n8nResourceLocator类的详细解释和分析。该类表示n8n资源定位器参数，具有一些属性和方法用于初始化和操作参数。在使用该类时，需要注意传入正确的参数，并根据需要调用相应的方法。
## FunctionDef __init__
**__init__ 函数**: 该函数的作用是初始化类实例。

该函数是一个构造器，用于初始化一个新的类实例。它接收一个参数 `param_json`，这个参数应该是一个字典，包含了初始化所需的参数。

在函数体内，首先调用了基类的构造器 `super().__init__(param_json)`，这是面向对象编程中常见的做法，用于确保基类也被正确地初始化。

接下来，函数定义了三个实例变量：
- `self.meta`：一个空字典，用于存储与参数相关的元数据。
- `self.mode`：一个空字符串，其具体用途在这段代码中没有给出，可能是用来指定某种模式或状态。
- `self.value`：初始化为 `None`，这个变量可能用来存储参数的值或其他相关信息。

**注意**：
- 在使用这个构造函数时，需要确保传入的 `param_json` 参数是一个字典，且包含了所有必要的初始化信息。
- 由于代码中没有提供 `super()` 基类的具体实现，因此在实际使用时，需要参考基类的文档或源代码，以确保正确地传入所需的参数。
- 实例变量 `meta`、`mode` 和 `value` 的具体用途和操作方式可能会在类的其他方法中有所体现，因此在使用这个类时，应该查阅相关的方法来了解如何操作这些变量。
## FunctionDef visit
**visit函数**: 该函数的功能是访问一个参数JSON并创建一个节点对象。

visit函数定义在`parameters.py`文件中，它是一个类方法，用于处理传入的参数JSON，创建并返回一个节点对象。这个过程中，它会检查参数JSON中是否包含关键字"modes"，如果不存在，则会触发断言错误。

具体来说，函数首先调用`n8nResourceLocator`函数来初始化一个节点对象。然后，它遍历参数JSON中的"modes"键对应的列表，对每一个实例，它会调用`visit_parameter`函数来访问参数实例。如果`visit_parameter`函数返回的结果不为空，那么这个结果会被添加到节点对象的`meta`属性中，并且设置其`father`属性为当前节点。

在项目中，`visit_parameter`函数负责根据参数的类型来访问参数，并返回相应的结果。它会检查参数JSON的"type"键，并根据不同的类型调用不同的visit方法，例如`n8nBoolean.visit`、`n8nNumber.visit`、`n8nString.visit`等。如果遇到未知的参数类型，它会打印出警告信息。

**注意**:
- 在使用visit函数之前，确保传入的参数JSON格式正确，并且包含了必要的"modes"键。
- visit函数依赖于`visit_parameter`函数及其他类型特定的visit方法，确保这些依赖函数都已正确实现。
- 如果参数JSON中的"modes"列表为空或者`visit_parameter`函数对所有实例都返回None，那么返回的节点对象的`meta`属性将为空。

**输出示例**:
假设有如下的参数JSON：
```json
{
    "modes": [
        {
            "name": "mode1",
            "type": "BOOLEAN",
            "value": true
        },
        {
            "name": "mode2",
            "type": "NUMBER",
            "value": 123
        }
    ]
}
```
调用`visit`函数后，可能返回的节点对象如下：
```python
Node(
    meta={
        "mode1": n8nBooleanNode(value=True, father=<Node>),
        "mode2": n8nNumberNode(value=123, father=<Node>)
    }
)
```
在这个示例中，`<Node>`代表节点对象本身的引用。
## FunctionDef refresh
**refresh 函数**: 此函数的功能是重置数据状态标志。

refresh 函数是一个简单的成员方法，它属于一个类（尽管具体类名在提供的代码片段中未给出）。此函数的主要作用是将类实例的 `data_is_set` 属性重置为 `False`。这通常表示与该属性相关的数据不再是最新的或尚未设置，需要在使用前进行更新或重新设置。

在实际应用中，这个函数可能被用于确保在执行某些操作之前，相关的数据是最新的。例如，如果类代表了一个配置参数集合，那么在参数发生变化后，可能需要调用 `refresh` 函数来标记参数需要被重新加载。

**注意**:
- 在调用此函数之前，确保了解其对类实例状态的影响，以及何时需要调用它来保持数据的准确性。
- 由于此函数将 `data_is_set` 设置为 `False`，在任何依赖于此标志的数据有效性检查之前，可能需要重新设置或更新数据。
- 此函数不接受任何参数，也不返回任何值。它的作用仅限于修改实例的内部状态。
## FunctionDef to_description
**to_description函数**: 该函数的功能是生成给定函数体的函数注释。

该`to_description`函数用于生成与特定参数相关的描述性文档字符串。它主要用于解析和展示参数的详细信息，包括参数的名称、类型、默认值、是否必须、描述以及与之相关的条件。

函数接收三个参数：
- `prefix_ids` (str): 前缀ID，用于标识参数的层级。
- `indent` (int): 缩进级别，默认为2。
- `max_depth` (int): 最大深度，默认为1。

函数首先通过调用`get_parameter_name`方法获取参数名称，并初始化一个空的列表`lines`来存储生成的注释行。

接着，函数构造了一个类型字符串`type_string`，表示参数的类型信息，这里固定为一个字典类型，其中包含一个`mode`字段和一个`values`字段。

函数创建了一个注释行`line1`，包含参数的名称、类型信息以及默认值（如果有的话）。如果参数有`display_string`，则会根据参数是否必须，添加相应的条件描述。如果参数被标记为必须，则简单地添加"Required"到描述中。

如果参数设置了`no_data_expression`标志，说明不能使用表达式，这也会被添加到描述中。最后，函数会列出`mode`字段可以接受的值。

如果参数的深度大于`max_depth`并且参数不是必须的，函数会添加一个隐藏的标记。否则，函数会递归地为每个`meta`中的键生成描述，并将这些描述添加到`lines`列表中。

最终，函数返回包含所有生成注释行的列表。

**注意**：
- 使用该函数时，需要确保传入正确的`prefix_ids`，以保证参数描述的准确性。
- `indent`和`max_depth`参数可以根据需要调整，以控制注释的格式和展示的详细程度。
- 函数的返回值是一个字符串列表，每个字符串代表一行注释，可以直接用于文档或代码中的注释部分。

**输出示例**：
假设有一个参数名为`param1`，其类型为字典，有一个默认值，不是必须的，且有一个描述字符串。那么`to_description`函数可能返回如下的列表：
```
[
  "  param1: dict{\"mode\":enum(str),\"values\":any}, Activate(Not Required) when (some condition), otherwise do not provide: Parameter description. \"mode\" should be one of ['option1', 'option2']: "
]
```
这个列表中的每一项都是一行注释，可以直接插入到代码的文档字符串中。
## FunctionDef to_json
**to_json 函数**: 该函数的功能是将对象转换为 JSON 表示形式。

该函数 `to_json` 负责将一个对象转换成其 JSON 格式的字典表示。这个转换过程中，会检查对象的 `data_is_set` 属性，以确定是否已经设置了数据。如果数据未设置，函数将返回 `None`，表示没有可转换的数据。

如果数据已经设置，函数会创建一个新的字典 `json_data`，并填充两个键值对：
- `"mode"` 键，其值为对象的 `mode` 属性，这通常是一个字符串，表示对象的模式。
- `"value"` 键，其值为对象的 `value` 属性的 JSON 表示。这里假设 `value` 属性本身就是一个对象，并且拥有自己的 `to_json` 方法，用于将 `value` 转换成其 JSON 表示形式。

最终，函数返回这个字典，即对象的 JSON 表示。

**注意**：
- 在使用这个函数之前，需要确保对象的 `data_is_set` 属性为真，否则函数将返回 `None`。
- 调用 `value` 属性的 `to_json` 方法时，需要保证该方法是存在且可用的，否则会抛出异常。

**输出示例**：
假设对象的 `mode` 属性值为 `"edit"`，`value` 属性是一个对象，其 `to_json` 方法返回的字典为 `{"text": "example"}`，那么 `to_json` 函数的返回值可能如下所示：
```python
{
    "mode": "edit",
    "value": {"text": "example"}
}
```
## FunctionDef parse_value
**parse_value函数**: 此函数的功能是解析给定的值，并返回一个包含工具调用状态和字符串消息的元组。

该函数`parse_value`是用于解析传入的值，并根据解析结果返回一个包含工具调用状态（`ToolCallStatus`）和字符串消息的元组。这个函数是`n8n_parser`模块中的一部分，用于处理n8n工作流中的参数值。

详细代码分析如下：

1. 函数接收一个名为`value`的参数，该参数可以是任何类型。
2. 函数首先检查`value`是否为字典类型，并且其键只包含"mode"和"value"。这是一种特定的参数格式，用于指定参数的模式和值。
3. 如果`value`满足上述条件，函数会进一步检查`value`字典中的"mode"键对应的值是否在当前对象的`meta`属性中定义。`meta`是一个字典，包含了所有支持的模式及其相关信息。
4. 如果"mode"的值未在`meta`中定义，函数将返回`ToolCallStatus.UndefinedParam`状态和一条错误消息，指出未定义的模式以及支持的模式列表。
5. 如果"mode"的值在`meta`中有定义，函数会深拷贝对应的模式信息，并调用该模式信息的`parse_value`方法来解析`value`字典中的"value"键对应的值。
6. 如果子参数的解析状态不是成功（`ToolCallStatus.ToolCallSuccess`），函数将返回子参数的解析状态和数据。
7. 如果子参数解析成功，函数会设置当前对象的`data_is_set`属性为True，记录所使用的模式到`mode`属性，并将解析后的模式信息保存到`value`属性。
8. 最后，函数返回`ToolCallStatus.ToolCallSuccess`状态和一条成功解析的消息。

如果`value`不满足特定的字典格式，函数将返回`ToolCallStatus.ParamTypeError`状态和一条错误消息，指出参数只能按照特定的格式解析，并提供了实际接收到的参数值的JSON格式字符串。

**注意**：
- 在使用`parse_value`函数时，需要确保传入的`value`参数格式正确，即为一个包含"mode"和"value"键的字典。
- 函数依赖于当前对象的`meta`属性，该属性需要预先定义所有支持的模式及其相关信息。
- 函数使用了`deepcopy`来避免在解析过程中修改原始的模式信息。

**输出示例**：
假设`meta`属性中定义了一个名为"example_mode"的模式，并且我们调用`parse_value`函数传入了以下参数：

```python
{
    "mode": "example_mode",
    "value": "some_value"
}
```

如果一切正常，函数可能返回如下的元组：

```python
(ToolCallStatus.ToolCallSuccess, "参数名 parsed with \"mode\"=example_mode")
```

如果传入的模式未在`meta`中定义，函数可能返回如下的元组：

```python
(ToolCallStatus.UndefinedParam, "Undefined mode \"example_mode\" for 参数名, supported modes: ['supported_mode1', 'supported_mode2']")
```

如果传入的`value`格式不正确，函数可能返回如下的元组：

```python
(ToolCallStatus.ParamTypeError, "参数名 can only be parsed as dict{\"mode\":str, \"value\":any}, got {\"incorrect_format\": true}")
```
***
