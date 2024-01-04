# ClassDef RecorderTypeEnum
**RecorderTypeEnum的功能**：该类的功能是定义了一系列用于XAgent运行记录器的枚举类型。

RecorderTypeEnum类为XAgent的运行记录器提供了一组预定义的字符串常量，这些常量用于标识记录器中不同类型的记录项。每个枚举值代表了记录器中可能存储的一种数据类型或配置信息。以下是每个枚举值的详细说明：

- `QUERY`：表示查询类型的记录，通常用于记录用户的查询请求。
- `CONFIG`：表示配置类型的记录，用于记录系统配置相关的信息。
- `LLM_INPUT_PAIR`：表示与大型语言模型（Large Language Model，简称LLM）相关的输入对记录。
- `TOOL_SERVER_PAIR`：表示与工具服务器相关的输入对记录。
- `NOW_SUBTASK_ID`：表示当前子任务的ID记录。
- `TOOL_CALL`：表示工具调用的记录。
- `PLAN_REFINE`：表示计划细化的记录。
- `LLM_SERVER_CACHE`：表示大型语言模型服务器缓存的记录。
- `TOOL_SERVER_CACHE`：表示工具服务器缓存的记录。
- `TOOL_CALL_CACHE`：表示工具调用缓存的记录。
- `PLAN_REFINE_CACHE`：表示计划细化缓存的记录。
- `LLM_INTERFACE_ID`：表示大型语言模型接口的ID记录。
- `TOOL_SERVER_INTERFACE_ID`：表示工具服务器接口的ID记录。
- `TOOL_CALL_ID`：表示工具调用的ID记录。

**注意**：在使用RecorderTypeEnum时，开发者应确保他们对应用程序中的记录类型有清晰的理解，并使用正确的枚举值来标识记录的类型。这有助于保持记录的一致性和可查询性。此外，开发者在添加新的记录类型时，应在RecorderTypeEnum中添加相应的枚举值，并在整个应用程序中保持一致的使用。
***
