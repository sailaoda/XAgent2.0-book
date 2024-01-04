# ClassDef XAgentConfig
**XAgentConfig 类的功能**：XAgentConfig 类是 XAgent 的配置类，用于存储和管理 XAgent 的各种配置信息。

**max_retry_times**: int = 3
最大重试次数，默认为 3。

**api_keys**: dict[str, list[dict]] = {
    "gpt-3.5-turbo-16k": [{
        "api_key": "sk-xxx",
        "model": "gpt-3.5-turbo-16k",
    }],
}
API 密钥配置，用于存储不同模型的 API 密钥信息。

**RequestConfig**: BaseModel
请求配置类，用于存储请求相关的配置信息。

- **lib**: Literal["openai", "xagent"] = 'openai'
    使用的库，可选值为 "openai" 或 "xagent"，默认为 "openai"。

- **format**: Literal[
    "chat",
    "tool_call",
    "function_call",
    "objling_link"] = "tool_call"
    请求的格式，可选值为 "chat"、"tool_call"、"function_call" 或 "objling_link"，默认为 "tool_call"。

- **json_mode**: bool = False
    是否以 JSON 格式发送请求，默认为 False。

- **schema_validation**: bool = True
    是否进行请求参数的模式验证，默认为 True。

- **default_model**: str = "gpt-3.5-turbo-16k"
    默认模型名称，默认为 "gpt-3.5-turbo-16k"。

- **default_timeout**: int = 600
    默认超时时间，默认为 600 秒。

**AdaEmbeddingConfig**: BaseModel
AdaEmbedding 配置类，用于存储 AdaEmbedding 相关的配置信息。

- **embedding_model**: str = "text-embedding-ada-002"
    嵌入模型名称，默认为 "text-embedding-ada-002"。

- **api_key**: str = ""
    API 密钥，默认为空。

- **base_url**: str = ""
    基础 URL，默认为空。

**ada_embedding**: AdaEmbeddingConfig = AdaEmbeddingConfig()
AdaEmbedding 配置，默认为 AdaEmbeddingConfig 类的实例。

**request**: RequestConfig = RequestConfig()
请求配置，默认为 RequestConfig 类的实例。

**ToolServerConfig**: BaseModel
ToolServer 配置类，用于存储 ToolServer 相关的配置信息。

- **url**: str = os.getenv("TOOLSERVER_URL", "http://localhost:8080")
    ToolServer 的 URL，默认为环境变量 TOOLSERVER_URL 的值，如果环境变量不存在，则默认为 "http://localhost:8080"。

- **cookies**: Optional[dict] = None
    请求的 cookies，默认为 None。

- **tool_blacklist**: list[str] = []
    工具的黑名单列表，默认为空列表。

**toolserver**: ToolServerConfig = ToolServerConfig()
ToolServer 配置，默认为 ToolServerConfig 类的实例。

**RecorderConfig**: BaseModel
记录器配置类，用于存储记录器相关的配置信息。

- **record_in_database**: bool = True
    是否将记录保存到数据库中，默认为 True。

- **logger**: Optional[str] = None
    日志记录器名称，默认为 None。

- **level**: Literal["DEBUG", "INFO", "WARNING", "ERROR", "CRITICAL"] = "INFO"
    日志记录级别，默认为 "INFO"。

- **reload**: bool = False
    是否重新加载记录，默认为 False。

- **reload_source**: Literal["database", "file"] = "file"
    重新加载记录的数据源，可选值为 "database" 或 "file"，默认为 "file"。

- **reload_id**: Optional[str] = None
    重新加载记录的 ID，默认为 None。

- **record_in_files**: bool = True
    是否将记录保存到文件中，默认为 True。

- **record_dir**: str = Field(
    default_factory=lambda: os.path.relpath(
        os.path.join(
            os.path.dirname(__file__),
            "..",
            "running_records",
            str(time.strftime('%Y_%m_%d_%H_%M_%S', time.localtime(
                int(round(time.time()))))) + '_' + uuid.uuid4().hex[:8]
        ))
)
    记录保存的目录，默认为运行时生成的时间戳和随机字符串组成的目录。

**recorder**: RecorderConfig = RecorderConfig()
记录器配置，默认为 RecorderConfig 类的实例。

**DatabasesConfig**: BaseModel
数据库配置类，用于存储数据库相关的配置信息。

**MongoDBConfig**: BaseModel
MongoDB 配置类，用于存储 MongoDB 相关的配置信息。

- **host**: str = os.getenv('MONGODB_HOST', 'localhost')
    MongoDB 主机地址，默认为环境变量 MONGODB_HOST 的值，如果环境变量不存在，则默认为 "localhost"。

- **port**: int = int(os.getenv('MONGODB_PORT', '27017'))
    MongoDB 端口号，默认为环境变量 MONGODB_PORT 的值，如果环境变量不存在，则默认为 27017。

- **username**: str = os.getenv('MONGODB_USERNAME', 'admin')
    MongoDB 用户名，默认为环境变量 MONGODB_USERNAME 的值，如果环境变量不存在，则默认为 "admin"。

- **password**: str = os.getenv('MONGODB_PASSWORD', '')
    MongoDB 密码，默认为空。

- **database**: str = os.getenv('MONGODB_DATABASE', 'XAgent')
    MongoDB 数据库名称，默认为环境变量 MONGODB_DATABASE 的值，如果环境变量不存在，则默认为 "XAgent"。

**mongodb**: MongoDBConfig = MongoDBConfig()
MongoDB 配置，默认为 MongoDBConfig 类的实例。

**RedisConfig**: BaseModel
Redis 配置类，用于存储 Redis 相关的配置信息。

- **host**: str = os.getenv('REDIS_HOST', 'localhost')
    Redis 主机地址，默认为环境变量 REDIS_HOST 的值，如果环境变量不存在，则默认为 "localhost"。

- **port**: int = int(os.getenv('REDIS_PORT', '6379'))
    Redis 端口号，默认为环境变量 REDIS_PORT 的值，如果环境变量不存在，则默认为 6379。

- **password**: str = os.getenv('REDIS_PASSWORD', '')
    Redis 密码，默认为空。

- **redis_db**: int = int(os.getenv('REDIS_DB', '0'))
    Redis 数据库索引，默认为环境变量 REDIS_DB 的值，如果环境变量不存在，则默认为 0。

- **vector_dim**: int = 1536
    向量维度，默认为 1536。

- **vector_type**: str = "FLOAT32"
    向量类型，默认为 "FLOAT32"。

- **distance_metric**: str = "COSINE"
    距离度量方式，默认为 "COSINE"。

**redis**: RedisConfig = RedisConfig()
Redis 配置，默认为 RedisConfig 类的实例。

**databases**: DatabasesConfig = DatabasesConfig()
数据库配置，默认为 DatabasesConfig 类的实例。

**ExectionConfig**: BaseModel
执行配置类，用于存储执行相关的配置信息。

- **enable_ask_human_for_help**: bool = False
    是否启用向人类请求帮助，默认为 False。

- **max_ai_functions_tokens**: int = 12800
    AI 函数的最大令牌数，默认为 12800。

- **max_message_tokens**: int = 15872
    消息的最大令牌数，默认为 15872。

- **max_chain_length**: int = 7
    链的最大长度，默认为 7。

- **max_plan_refine_chain_length**: int = 3
    计划细化链的最大长度，默认为 3。

- **max_plan_tree_depth**: int = 3
    计划树的最大深度，默认为 3。

- **max_plan_tree_width**: int = 5
    计划树的最大宽度，默认为 5。

- **max_tool_retrieve_count**: int = 0
    工具检索的最大数量，默认为 0。

**SummaryConfig**: BaseModel
摘要配置类，用于存储摘要相关的配置信息。

- **enable**: bool = True
    是否启用摘要功能，默认为 True。

- **single_step_max_tokens**: int = 2048
    单步摘要的最大令牌数，默认为 2048。

- **max_return_tokens**: int = 12384
    摘要返回的最大令牌数，默认为 12384。

- **tool_calls**: bool = True
    是否摘要工具调用信息，默认为 True。

- **working_context**: bool = True
    是否摘要工作上下文信息，默认为 True。

**summary**: SummaryConfig = SummaryConfig()
摘要配置，默认为 SummaryConfig 类的实例。

**SelfEvolvingConfig**: BaseModel
自进化配置类，用于存储自进化相关的配置信息。

- **enable_online_plan_consolidate**: bool = False
    是否启用在线计划整合，默认为 False。

- **enable_plan_save**: bool = False
    是否保存计划，默认为 False。

- **enable_online_inner_consolidate**: bool = False
    是否启用在线内部整合，默认为 False。

- **enable_inner_save**: bool = False
    是否保存内部，默认为 False。

- **inner_save_path**: str = "local_workspace/self_evolve_inner"
    内部保存路径，默认为 "local_workspace/self_evolve_inner"。

- **plan_save_path**: str = "local_workspace/self_evolve_plan"
    计划保存路径，默认为 "local_workspace/self_evolve_plan"。

- **enable_plan_retrieve**: bool = False
    是否启用计划检索，默认为 False。

- **enable_inner_retrieve**: bool = False
    是否启用内部检索，默认为 False。

- **plan_namespace**: str = "evolve_plan"
    计划的命名空间，默认为 "evolve_plan"。

- **inner_namespace**: str = "evolve_inner_1208"
    内部的命名空间，默认为 "evolve_inner_1208"。

**evolve**: SelfEvolvingConfig = SelfEvolvingConfig()
自进化配置，默认为 SelfEvolvingConfig 类的实例。

**PipelineConfig**: BaseModel
流水线配置类，用于存储流水线相关的配置信息。

- **data_root**: str = "local_workspace/pipelines/"
    数据根目录，默认为 "local_workspace/pipelines/"。

- **pipeline_namespace**: str = ""
    流水线的命名空间，默认为空字符串。

**pipeline**: PipelineConfig = PipelineConfig()
流水线配置，默认为 PipelineConfig 类的实例。

**signal_interrupt**: bool = False
信号中断，默认为 False。

**execution**: ExectionConfig = ExectionConfig()
执行配置，默认为 ExectionConfig 类的实例。

**MultiModalConfig**: BaseModel
多模态配置类，用于存储多模态相关的配置信息。

**ImageModalConfig**: BaseModel
图像模态配置类，用于存储图像模态相关的配置信息。

- **enable**: bool = False
    是否启用图像模态，默认为 False。

- **model**: str = "gpt-4-vision"
    图像模型名称，默认为 "gpt-4-vision"。

**image**: ImageModalConfig = ImageModalConfig()
图像模态配置，默认为 ImageModalConfig 类的实例。

**multimodal**: MultiModalConfig = MultiModalConfig()
多模态配置，默认为 MultiModalConfig 类的实例。

**ExperimentConfig**: BaseModel
实验配置类，用于存储实验相关的配置信息。

- **disable_inital_plan_generation**: bool = False
    是否禁用初始计划生成，默认为 False。

- **maintain_history_context**: bool = False
    是否保留历史上下文，默认为 False。

- **use_thread_engine**: bool = False
    是否使用线程引擎，默认为 False。

- **redo_action**: bool = False
    是否重新执行动作，默认为 False。

**experiment**: ExperimentConfig = ExperimentConfig()
实验配置，默认为 ExperimentConfig 类的实例。

**RapidapiEnvConfig**: BaseModel
RapidAPI 环境配置类，用于存储 RapidAPI 相关的配置信息。

- **id2tool_file**: str = "assets/rapidapi/id2tool.json"
    ID 到工具文件的路径，默认为 "assets/rapidapi/id2tool.json"。

- **doc_embedding_file**: str = "assets/rapidapi/doc_embedding.npy"
    文档嵌入文件的路径，默认为 "assets/rapidapi/doc_embedding.npy"。

- **tool_info_file**: str = "assets/rapidapi/tool_info.json"
    工具信息文件的路径，默认为 "assets/rapidapi/tool_info.json"。

- **api_key**: str = ""
    API 密钥，默认为空。

- **endpoint**: str = "http://8.218.239.54:8080/fake_rapidapi"
    终端点，默认为 "http://8.218.239.54:8080/fake_rapidapi"。

- **timeout**: int = 600
    超时时间，默认为 600 秒。

**rapidapi**: RapidapiEnvConfig = RapidapiEnvConfig()
RapidAPI 环境配置，默认为 RapidapiEnvConfig 类的实例。

**to_dict()**: dict
将配置转换为字典形式。

**reload(config_file=None)**: None
重新加载配置。

- **config_file**: str, optional
    配置文件路径，默认为 None。

**set_value_with_args(args=ARGS)**: None
根据命令行参数设置配置值。

- **args**: dict, optional
    命令行参数，默认为 ARGS。

**get_model_name(model_name: str = None) -> str**: 
获取模型的标准化名称。

- **model_name**: str, optional
    模型名称，默认为 None。

**get_apiconfig_by_model(model_name: str) -> dict**: 
根据模型名称获取 API 配置。

- **model_name**: str
    模型名称。

**注意**：该类用于存储和管理 XAgent 的各种配置信息，包括请求配置、工具服务器配置、记录器配置、数据库配置、执行配置等。可以通过调用 `reload` 方法重新加载配置，也可以通过调用 `set_value_with_args` 方法根据命令行参数设置配置值。

**输出示例**：
```python
config = XAgentConfig()
print(config.max_retry_times)  # 3

print(config.request.lib)  # 'openai'
print(config.request.format)  # 'tool_call'

print(config.toolserver.url)  # 'http://localhost:8080'

print(config.recorder.record_in_database)  # True

print(config.databases.mongodb.host)  # 'localhost'
print(config.databases.mongodb.port)  # 27017

print(config.execution.max_ai_functions_tokens)  # 12800

print(config.multimodal.image.enable)  # False

print(config.experiment.disable_inital_plan_generation)  # False

print(config.rapidapi.id2tool_file)  # 'assets/rapidapi/id2tool.json'
```
## ClassDef RequestConfig
**RequestConfig类的功能**：RequestConfig类用于配置请求的相关参数，包括请求的库、格式、JSON模式、模式验证等。

RequestConfig类具有以下属性：

- `lib`：请求的库，取值为`"openai"`或`"xagent"`，默认值为`"openai"`。
- `format`：请求的格式，取值为`"chat"`、`"tool_call"`、`"function_call"`或`"objling_link"`，默认值为`"tool_call"`。
- `json_mode`：是否使用JSON模式发送请求，布尔类型，默认值为`False`。
- `schema_validation`：是否进行模式验证，布尔类型，默认值为`True`。

RequestConfig类还包含一个内部类`AdaEmbeddingConfig`，用于配置AdaEmbedding相关参数，包括嵌入模型、API密钥和基础URL。

- `embedding_model`：嵌入模型，默认值为`"text-embedding-ada-002"`。
- `api_key`：API密钥，默认为空字符串。
- `base_url`：基础URL，默认为空字符串。

**注意**：在使用RequestConfig类时，可以根据需要设置相应的属性值。
### ClassDef AdaEmbeddingConfig
**AdaEmbeddingConfig 类的功能**: 此类的功能是配置用于文本嵌入的 Ada 模型的参数。

AdaEmbeddingConfig 类是一个配置类，它继承自 BaseModel，用于设置和存储与 Ada 文本嵌入模型相关的配置信息。这个类定义了三个属性：

1. `embedding_model`: 字符串类型，默认值为 `"text-embedding-ada-002"`。这个属性指定了要使用的嵌入模型的名称。在这个上下文中，它被设置为使用 OpenAI 提供的 Ada 模型的特定版本。

2. `api_key`: 字符串类型，默认值为空字符串。这个属性用于存储访问模型所需的 API 密钥。用户需要根据自己的账户信息填写这个字段以便能够调用相应的 API。

3. `base_url`: 字符串类型，默认值为空字符串。这个属性用于存储 API 的基础 URL。用户需要根据 API 提供方的要求填写这个字段以确保能够正确地访问服务。

在项目中，AdaEmbeddingConfig 类被嵌套在 RequestConfig 类中，并且实例化了一个名为 `ada_embedding` 的 AdaEmbeddingConfig 对象。这表明 AdaEmbeddingConfig 类的实例将作为 RequestConfig 类的一个属性来使用，允许在构建请求时轻松访问和使用 Ada 模型的配置信息。

**注意**：
- 在使用 AdaEmbeddingConfig 类时，开发者需要确保 `api_key` 和 `base_url` 被正确设置，以便能够成功调用 API。
- 默认的嵌入模型 `embedding_model` 已经预设为 `"text-embedding-ada-002"`，但如果需要使用不同版本的模型，开发者可以修改此属性。
- 由于 AdaEmbeddingConfig 类是 BaseModel 的子类，它可以自动利用 Pydantic 库的功能，例如自动类型检查和数据验证，以确保配置数据的正确性。
- 在实际部署和使用时，应确保敏感信息如 `api_key` 不要硬编码在代码中，而是通过环境变量或配置文件安全地管理。
## ClassDef ToolServerConfig
**ToolServerConfig函数**：这个类的功能是定义ToolServer的配置。

ToolServerConfig类是XAgentConfig类的一个内部类，用于定义ToolServer的配置信息。它继承自BaseModel类，具有以下属性：

- url: str类型，表示ToolServer的URL地址，默认值为环境变量TOOLSERVER_URL的值，如果环境变量不存在，则默认为"http://localhost:8080"。
- cookies: Optional[dict]类型，表示ToolServer的cookies信息，默认值为None。
- tool_blacklist: list[str]类型，表示ToolServer的工具黑名单列表，默认值为空列表。

在XAgentConfig类中，通过toolserver属性来引用ToolServerConfig类，并将其实例化为ToolServerConfig对象。

在XAgent/config.py文件中，ToolServerConfig类被用于定义XAgentConfig类的toolserver属性，用于配置ToolServer的相关信息。

**注意**：在使用ToolServer时，可以通过修改ToolServerConfig类的属性来自定义ToolServer的URL地址、cookies信息和工具黑名单列表。
## ClassDef RecorderConfig
**RecorderConfig函数**：这个类的功能是定义记录器的配置。

RecorderConfig类是XAgent配置文件中的一部分，用于配置记录器的行为。它包含了以下属性：

- record_in_database: 一个布尔值，表示是否将记录保存到数据库中，默认为True。
- logger: 一个可选的字符串，表示记录器的名称，默认为None。
- level: 一个字符串，表示记录器的日志级别，可选值为"DEBUG"、"INFO"、"WARNING"、"ERROR"和"CRITICAL"，默认为"INFO"。
- reload: 一个布尔值，表示是否重新加载记录，默认为False。
- reload_source: 一个字符串，表示重新加载记录的来源，目前只支持"database"和"file"两种方式，默认为"file"。
- reload_id: 一个可选的字符串，表示重新加载记录的ID，默认为None。
- record_in_files: 一个布尔值，表示是否将记录保存到文件中，默认为True。
- record_dir: 一个字符串，表示记录保存的目录，默认为一个根据当前时间和随机数生成的目录。

这个类的作用是定义记录器的配置，开发者可以根据自己的需求来配置记录器的行为。通过设置不同的属性值，可以控制记录器的保存方式、日志级别等。

**注意**：在使用这个类时，开发者需要注意以下几点：
- 默认情况下，记录器会将记录保存到数据库中，并且保存到文件中。
- 如果需要重新加载记录，可以将reload属性设置为True，并指定重新加载的来源和ID。
- 开发者可以根据自己的需求，修改record_dir属性来指定记录保存的目录。
- 如果需要更改记录器的日志级别，可以修改level属性的值。

这个类是XAgent配置文件中的一部分，用于配置记录器的行为。开发者可以根据自己的需求来配置记录器的保存方式、日志级别等。
## ClassDef DatabasesConfig
**DatabasesConfig类的功能**：DatabasesConfig类是XAgent配置文件中的一个子类，用于配置数据库的相关参数。

DatabasesConfig类包含两个内部类：MongoDBConfig和RedisConfig，分别用于配置MongoDB和Redis数据库的参数。

**MongoDBConfig类的功能**：MongoDBConfig类用于配置MongoDB数据库的相关参数，包括主机名、端口号、用户名、密码和数据库名称等。

**RedisConfig类的功能**：RedisConfig类用于配置Redis数据库的相关参数，包括主机名、端口号、密码、数据库编号、向量维度、向量类型和距离度量等。

**注意**：在使用DatabasesConfig类时，可以根据需要修改MongoDBConfig和RedisConfig的参数，以满足具体的数据库配置需求。
### ClassDef MongoDBConfig
**MongoDBConfig 类的功能**：该类的功能是配置和管理MongoDB数据库的连接信息。

MongoDBConfig 类是一个配置类，它继承自 BaseModel，用于定义和存储MongoDB数据库的连接参数。这些参数包括主机地址、端口号、用户名、密码以及数据库名称。这些参数的默认值可以通过环境变量进行设置，如果没有设置环境变量，则会使用预设的默认值。

具体来说，MongoDBConfig 类包含以下属性：
- `host`：字符串类型，表示MongoDB服务器的主机地址。如果没有设置环境变量 `MONGODB_HOST`，默认值为 `'localhost'`。
- `port`：整型，表示MongoDB服务的端口号。如果没有设置环境变量 `MONGODB_PORT`，默认值为 `27017`。
- `username`：字符串类型，表示连接MongoDB时使用的用户名。如果没有设置环境变量 `MONGODB_USERNAME`，默认值为 `'admin'`。
- `password`：字符串类型，表示连接MongoDB时使用的密码。如果没有设置环境变量 `MONGODB_PASSWORD`，默认值为空字符串 `''`。
- `database`：字符串类型，表示要连接的MongoDB数据库的名称。如果没有设置环境变量 `MONGODB_DATABASE`，默认值为 `'XAgent'`。

在项目中，MongoDBConfig 类被实例化并赋值给 DatabasesConfig 类的 `mongodb` 属性。这样，当需要连接到MongoDB数据库时，可以通过 DatabasesConfig 的实例来获取MongoDB的配置信息。

**注意**：
- 在使用MongoDBConfig 类时，应确保相关的环境变量已经正确设置，以便能够连接到正确的MongoDB实例。
- 如果在部署环境中MongoDB的连接信息有所不同，应在环境变量中进行相应的设置，而不是直接修改代码中的默认值。
- 由于密码等敏感信息通常不应硬编码在代码中，建议使用环境变量来管理这些敏感信息。
- BaseModel 类通常来自于 Pydantic 库，它提供了数据验证和管理的功能，确保配置信息的类型正确且符合预期。在使用前需要确保已经安装了 Pydantic 库。
### ClassDef RedisConfig
**RedisConfig 类的功能**：该类的功能是定义和配置Redis数据库的连接参数。

RedisConfig 类是一个配置类，它继承自 BaseModel，用于设置和存储连接到Redis数据库所需的各种配置信息。这些配置信息包括Redis服务器的主机地址、端口号、密码、数据库编号以及与向量搜索相关的配置，如向量维度、向量类型和距离度量方式。

具体来说，RedisConfig 类包含以下字段：

- `host`: 字符串类型，表示Redis服务器的主机地址。默认值从环境变量 `REDIS_HOST` 获取，如果未设置，则默认为 `'localhost'`。
- `port`: 整型，表示Redis服务器的端口号。默认值从环境变量 `REDIS_PORT` 获取，如果未设置，则默认为 `6379`。
- `password`: 字符串类型，表示连接Redis服务器所需的密码。默认值从环境变量 `REDIS_PASSWORD` 获取，如果未设置，则默认为空字符串 `''`。
- `redis_db`: 整型，表示要连接的Redis数据库编号。默认值从环境变量 `REDIS_DB` 获取，如果未设置，则默认为 `0`。
- `vector_dim`: 整型，表示向量的维度，用于向量搜索。默认值为 `1536`。
- `vector_type`: 字符串类型，表示向量的数据类型，用于向量搜索。默认值为 `"FLOAT32"`。
- `distance_metric`: 字符串类型，表示向量搜索时使用的距离度量方式。默认值为 `"COSINE"`。

在项目中，RedisConfig 类被嵌套在 DatabasesConfig 类中，并被实例化为 `redis` 属性。这样，可以通过 `DatabasesConfig.redis` 方便地访问到Redis的配置信息。

**注意**：
- 在使用 RedisConfig 类时，需要确保相关的环境变量已经正确设置，否则将使用默认值。
- 如果项目中需要对Redis进行向量搜索，那么 `vector_dim`、`vector_type` 和 `distance_metric` 这三个字段将非常重要，它们需要根据实际使用的向量搜索引擎和需求进行配置。
- 由于 RedisConfig 类继承自 BaseModel，它可以自动进行类型检查和验证，确保配置信息的正确性。
- 在部署到不同环境时（如开发环境、测试环境和生产环境），可能需要根据环境不同而调整环境变量的值，以确保连接到正确的Redis实例。
## ClassDef ExectionConfig
**ExectionConfig函数**：该类的功能是定义执行配置。

ExectionConfig类是XAgent配置文件中的一个子类，用于定义执行配置的各种参数。它包含了一系列的属性，用于控制XAgent的执行行为。

- enable_ask_human_for_help: 是否启用人工帮助，默认为False。

- max_ai_functions_tokens: AI函数的最大令牌数，默认为12800。

- max_message_tokens: 消息的最大令牌数，默认为15872。

- max_chain_length: 链的最大长度，默认为7。

- max_plan_refine_chain_length: 计划细化链的最大长度，默认为3。

- max_plan_tree_depth: 计划树的最大深度，默认为3。

- max_plan_tree_width: 计划树的最大宽度，默认为5。

- max_tool_retrieve_count: 工具检索的最大数量，默认为0。

- summary: 摘要配置，包含了一系列用于生成摘要的参数。

  - enable: 是否启用摘要，默认为True。

  - single_step_max_tokens: 单步摘要的最大令牌数，默认为2048。

  - max_return_tokens: 摘要的最大返回令牌数，默认为12384。

  - tool_calls: 是否包含工具调用信息，默认为True。

  - working_context: 是否包含工作环境信息，默认为True。

- evolve: 自我进化配置，包含了一系列用于自我进化的参数。

  - enable_online_plan_consolidate: 是否启用在线计划合并，默认为False。

  - enable_plan_save: 是否保存计划，默认为False。

  - enable_online_inner_consolidate: 是否启用在线内部合并，默认为False。

  - enable_inner_save: 是否保存内部，默认为False。

  - inner_save_path: 内部保存路径，默认为"local_workspace/self_evolve_inner"。

  - plan_save_path: 计划保存路径，默认为"local_workspace/self_evolve_plan"。

  - enable_plan_retrieve: 是否检索计划，默认为False。

  - enable_inner_retrieve: 是否检索内部，默认为False。

  - plan_namespace: 计划命名空间，默认为"evolve_plan"。

  - inner_namespace: 内部命名空间，默认为"evolve_inner_1208"。

- pipeline: 流水线配置，包含了一系列用于流水线的参数。

  - data_root: 数据根目录，默认为"local_workspace/pipelines/"。

  - pipeline_namespace: 流水线命名空间，默认为空。

- signal_interrupt: 是否启用中断信号，默认为False。

**注意**：在使用ExectionConfig类时，可以根据需要修改各个属性的值来调整XAgent的执行行为。

**输出示例**：
```python
config = ExectionConfig()
print(config.enable_ask_human_for_help)  # False
print(config.max_ai_functions_tokens)  # 12800
print(config.summary.enable)  # True
print(config.evolve.enable_online_plan_consolidate)  # False
print(config.pipeline.data_root)  # "local_workspace/pipelines/"
```
### ClassDef SummaryConfig
**SummaryConfig功能**: 此类的功能是配置摘要生成的相关参数。

SummaryConfig类是一个配置类，它继承自BaseModel，用于设置和管理摘要生成过程中的一些关键参数。这个类定义在XAgent项目的config.py文件中，作为ExectionConfig类的一个内嵌类。它的主要作用是控制摘要生成的行为和限制。

详细代码分析如下：

- `enable`: 类型为bool，默认值为True。此参数用来开启或关闭摘要生成功能。当设置为True时，摘要生成功能将被激活。

- `single_step_max_tokens`: 类型为int，默认值为2048。此参数用来设置单步操作中最大的token数量。在生成摘要的过程中，每一步操作所能使用的最大token数由此参数控制。

- `max_return_tokens`: 类型为int，默认值为12384。此参数用来设置返回摘要的最大token数量。生成的摘要总长度不会超过此参数设定的值。

- `tool_calls`: 类型为bool，默认值为True。此参数用来决定是否在摘要中包含对工具调用的信息。

- `working_context`: 类型为bool，默认值为True。此参数用来决定是否在摘要中包含工作上下文的信息。

在项目中，SummaryConfig类被实例化为ExectionConfig类的一个属性`summary`。这意味着，当需要配置摘要生成相关的参数时，可以通过修改ExectionConfig实例的summary属性来实现。

**注意**：
- 在使用SummaryConfig类时，需要确保其父类ExectionConfig也被正确配置和实例化。
- 修改SummaryConfig类中的参数时，应当注意参数的类型和默认值，确保设置的值在合理范围内，以免影响摘要生成的效果和性能。

**输出示例**：
```python
summary_config = SummaryConfig(
    enable=True,
    single_step_max_tokens=2048,
    max_return_tokens=12384,
    tool_calls=True,
    working_context=True
)
```
以上代码展示了如何实例化一个SummaryConfig对象，并设置了所有参数。在实际应用中，可以根据需要调整这些参数值。
### ClassDef SelfEvolvingConfig
**SelfEvolvingConfig功能**: 该类的功能是配置自我演化系统的相关参数。

SelfEvolvingConfig类是一个配置类，它继承自BaseModel，用于设置和管理自我演化系统的行为。这个类包含了多个布尔型的配置项和字符串型的路径配置，用于控制自我演化过程中的数据保存、检索以及在线合并等功能。

- `enable_online_plan_consolidate`: 布尔型，表示是否启用在线计划合并功能。当设置为True时，系统将尝试在线合并计划，以优化执行效率。
- `enable_plan_save`: 布尔型，表示是否启用计划保存功能。当设置为True时，系统将会保存演化过程中生成的计划数据。
- `enable_online_inner_consolidate`: 布尔型，表示是否启用在线内部合并功能。当设置为True时，系统将在线合并内部数据，以提高数据处理效率。
- `enable_inner_save`: 布尔型，表示是否启用内部数据保存功能。当设置为True时，系统将保存内部演化过程中的数据。
- `inner_save_path`: 字符串型，表示内部数据保存的路径。默认值为"local_workspace/self_evolve_inner"。
- `plan_save_path`: 字符串型，表示计划数据保存的路径。默认值为"local_workspace/self_evolve_plan"。
- `enable_plan_retrieve`: 布尔型，表示是否启用计划数据检索功能。当设置为True时，系统将尝试检索并使用之前保存的计划数据。
- `enable_inner_retrieve`: 布尔型，表示是否启用内部数据检索功能。当设置为True时，系统将尝试检索并使用之前保存的内部数据。
- `plan_namespace`: 字符串型，表示计划数据的命名空间。默认值为"evolve_plan"。
- `inner_namespace`: 字符串型，表示内部数据的命名空间。默认值为"evolve_inner_1208"。

在项目中，SelfEvolvingConfig类被实例化为`evolve`对象，并作为`ExectionConfig`类的一个属性。这表明自我演化的配置是执行配置的一部分，并且可以通过`ExectionConfig`的实例来访问和修改自我演化的相关设置。

**注意**：
- 在使用SelfEvolvingConfig类时，开发者需要根据实际需求来设置各项配置，以确保自我演化系统的行为符合预期。
- 需要注意的是，保存路径配置项（如`inner_save_path`和`plan_save_path`）应指向有效的文件系统路径，以确保数据能够正确保存。
- 当启用数据保存功能时（如`enable_plan_save`和`enable_inner_save`），应确保有足够的磁盘空间来存储生成的数据。
- 启用数据检索功能（如`enable_plan_retrieve`和`enable_inner_retrieve`）时，需要确保之前保存的数据是有效且格式正确的，否则可能会影响系统的正常运行。
### ClassDef PipelineConfig
**PipelineConfig 类的功能**：该类用于定义管道配置的数据结构。

PipelineConfig 类继承自 BaseModel，用于存储与管道相关的配置信息。它包含两个属性：data_root 和 pipeline_namespace。

- `data_root` 属性是一个字符串，默认值为 "local_workspace/pipelines/"。这个属性指定了管道数据的根目录，所有与管道相关的数据都将存储在这个目录下。这个目录可以是相对路径，也可以是绝对路径，取决于项目的具体部署环境。

- `pipeline_namespace` 属性也是一个字符串，默认值为空字符串。这个属性用于定义管道的命名空间，它可以帮助区分不同的管道配置，特别是在有多个管道并存时。如果设置了命名空间，相关的管道数据可能会存储在以命名空间命名的子目录中。

在 XAgent/config.py 文件中，PipelineConfig 类被实例化为 `pipeline` 对象，并作为 `ExectionConfig` 类的一个属性。这表明管道配置是执行配置的一部分，执行配置可能包含了多个不同方面的设置，管道配置只是其中之一。

**注意**：
- 在使用 PipelineConfig 类时，开发者应确保 `data_root` 属性指向的目录是有效的，并且应用程序有足够的权限来读写该目录。
- 如果需要对管道进行命名空间隔离，应合理设置 `pipeline_namespace` 属性。如果不需要隔离，可以保持默认的空字符串。
- 由于 PipelineConfig 类是 BaseModel 的子类，它支持 Pydantic 库提供的所有特性，包括数据验证、序列化和反序列化等。开发者可以利用这些特性来确保配置数据的正确性和易用性。
## ClassDef MultiModalConfig
**MultiModalConfig类的功能**：该类用于配置多模态功能。

MultiModalConfig类是XAgentConfig类的一个内部类，用于配置多模态功能。它包含一个内部类ImageModalConfig，用于配置图像模态的相关参数。

**ImageModalConfig类的功能**：该类用于配置图像模态的相关参数。

ImageModalConfig类是MultiModalConfig类的一个内部类，用于配置图像模态的相关参数。它包含两个属性：enable和model。enable属性用于指示是否启用图像模态，默认为False；model属性用于指定使用的图像模型，默认为"gpt-4-vision"。

**注意**：在使用MultiModalConfig类时，可以根据需要修改ImageModalConfig类的属性值来配置图像模态的相关参数。
### ClassDef ImageModalConfig
**ImageModalConfig功能**: 该类的功能是定义图像模态配置。

ImageModalConfig类是一个配置类，它继承自BaseModel，用于定义图像模态的相关配置。这个类包含两个属性：`enable`和`model`。

- `enable`: 这是一个布尔类型的属性，默认值为`False`。它用于指示是否启用图像模态功能。如果设置为`True`，则表示启用图像模态；如果为`False`，则表示不启用。
- `model`: 这是一个字符串类型的属性，默认值为`"gpt-4-vision"`。它用于指定使用的图像模型的名称。在这里，它被设置为使用`gpt-4-vision`模型，这可能是一个预定义的模型名称，用于图像处理或图像识别任务。

在项目中，ImageModalConfig类被嵌套在另一个名为MultiModalConfig的类中。这表明ImageModalConfig类是多模态配置的一部分，专门负责图像模态的配置。在MultiModalConfig类中，创建了一个名为`image`的ImageModalConfig实例，这样就可以通过`MultiModalConfig.image`来访问和修改图像模态的配置。

**注意**:
- 在使用ImageModalConfig类时，开发者应确保正确设置`enable`属性以启用或禁用图像模态功能。
- `model`属性应根据项目需求设置为合适的模型名称，以确保图像处理功能的正确执行。
- 由于ImageModalConfig类是通过BaseModel继承而来，它支持Pydantic库提供的数据验证和序列化功能，这有助于确保配置数据的正确性和一致性。
## ClassDef ExperimentConfig
**ExperimentConfig类的功能**：该类用于配置实验参数。

ExperimentConfig类是XAgent配置文件中的一个子类，用于配置实验相关的参数。该类继承自BaseModel类，具有以下属性：

- disable_inital_plan_generation: bool类型，表示是否禁用初始计划生成，默认为False。
- maintain_history_context: bool类型，表示是否保留历史上下文，默认为False。
- use_thread_engine: bool类型，表示是否使用线程引擎，默认为False。
- redo_action: bool类型，表示是否重新执行动作，默认为False。

该类的作用是为XAgent提供实验相关的配置参数。通过配置这些参数，可以灵活地控制实验的行为和结果。

**注意**：在使用ExperimentConfig类时，可以根据实际需求配置disable_inital_plan_generation、maintain_history_context、use_thread_engine和redo_action等参数，以达到预期的实验效果。
## ClassDef RapidapiEnvConfig
**RapidapiEnvConfig类的功能**：RapidapiEnvConfig类是XAgent配置文件中的一个子类，用于配置RapidAPI环境的相关参数。

RapidapiEnvConfig类具有以下属性：
- id2tool_file: str类型，表示id2tool文件的路径，默认值为"assets/rapidapi/id2tool.json"。该文件用于存储工具的ID和名称的映射关系。
- doc_embedding_file: str类型，表示doc_embedding文件的路径，默认值为"assets/rapidapi/doc_embedding.npy"。该文件用于存储文档的嵌入向量。
- tool_info_file: str类型，表示tool_info文件的路径，默认值为"assets/rapidapi/tool_info.json"。该文件用于存储工具的详细信息。
- api_key: str类型，表示Toolbench的API密钥，默认值为空字符串。用于访问RapidAPI工具。
- endpoint: str类型，表示RapidAPI的终端点，默认值为"http://8.218.239.54:8080/fake_rapidapi"。用于发送请求到RapidAPI。
- timeout: int类型，表示请求的超时时间，默认值为600秒。

**注意**：在使用RapidAPI环境时，可以根据需要修改这些参数的值。
## FunctionDef to_dict
**to_dict函数**：该函数的功能是将对象转换为字典形式。

该函数是一个实例方法，用于将对象转换为字典形式。它调用了对象的model_dump方法，并传入了一个exclude参数，用于指定在转换过程中需要排除的属性。该函数返回转换后的字典。

调用该函数的代码如下所示：

在文件XAgent/record.py中的函数_write_config中调用了to_dict函数。该函数用于更新XAgent的配置。在函数中，首先判断是否需要将配置信息记录到文件中，如果需要，则将配置信息以YAML格式写入到文件中。然后，将配置信息赋值给record对象的config属性，并保存到数据库中。

在文件XAgentServer/server.py中的interact函数中调用了to_dict函数。该函数用于启动XAgent Server。在函数中，首先重新加载配置信息，然后根据传入的参数构建XAgentParam对象，并调用to_dict函数将配置信息转换为字典形式。最后，将文件列表中的文件上传到XAgent Core组件中。

在文件main.py中的run函数中调用了to_dict函数。该函数用于运行XAgent。在函数中，首先设置配置信息，然后初始化数据库和ToolServer接口。接着，根据传入的参数构建XAgentParam对象，并根据不同的workflow类型选择不同的工作流程进行处理。

在文件tests/test_pipeline.py中的run函数中调用了to_dict函数。该函数用于运行XAgent的Pipeline工作流程。在函数中，首先设置配置信息，然后上传文件到ToolServer接口。接着，初始化数据库和Beanie，并根据传入的参数选择不同的工作流程进行处理。

在文件tests/test_plan_pipeline_loop.py中的run函数中调用了to_dict函数。该函数用于运行XAgent的PlanPipelineLoop工作流程。在函数中，首先设置配置信息，然后上传文件到ToolServer接口。接着，初始化数据库和Beanie，并根据传入的参数选择不同的工作流程进行处理。

**注意**：在使用该函数时，需要注意传入的exclude参数，以确保转换后的字典不包含指定的属性。

**输出示例**：假设对象的属性为{'name': 'John', 'age': 25, 'gender': 'male'}，调用to_dict函数后返回的字典为{'name': 'John', 'age': 25}。
## FunctionDef reload
**reload函数**: 此函数的功能是重新加载配置文件。

reload函数是一个用于重新加载配置文件的方法。它允许开发者在运行时动态更新XAgent的配置，而无需重启应用程序。这在开发过程中特别有用，因为它可以节省时间并提高效率。

函数首先检查是否提供了config_file参数。如果没有提供，它将尝试从环境变量CONFIG_FILE中获取配置文件路径，默认为"assets/config.yml"。然后，它使用yaml库加载配置文件，并创建一个新的XAgentConfig实例，使用配置文件中的数据初始化这个实例。

接下来，函数会重新初始化自身，使用新配置实例的model_dump方法返回的字典作为参数。这样做的目的是将当前对象的状态更新为新的配置状态。

此外，函数还会检查环境变量TOOLSERVER_URL，并更新self.toolserver.url属性，如果环境变量中有相应的值的话。

如果全局变量ARGS（假设是在其他地方定义的命令行参数列表）不为空，函数会打印出这些参数，并调用set_value_with_args方法来根据这些参数更新配置。

在项目中，reload函数被调用于XAgentServer/server.py文件中的interact方法。在这个上下文中，reload函数用于在处理交互请求之前，确保XAgent的配置是最新的。通过调用config.reload()，确保了任何环境变量或外部变更的配置文件都会被加载和应用。

**注意**:
- 在使用reload函数时，需要确保配置文件的路径正确无误，且配置文件的格式符合yaml规范。
- 如果配置文件中的数据结构与XAgentConfig类的构造函数所需的参数不匹配，可能会引发错误。
- 在调用reload函数之前，应该确保所有必要的环境变量已经设置，特别是如果配置依赖于这些环境变量的话。
- 考虑到reload函数会打印出配置文件路径和ARGS变量，如果涉及敏感信息，应当注意日志的安全性和隐私性。
- 在多线程或多进程环境中使用reload函数时，需要注意同步和并发问题，确保配置的重新加载不会导致状态不一致或竞争条件。
## FunctionDef set_value_with_args
**set_value_with_args函数**：此函数的功能是根据传入的参数设置配置值。

该函数接受一个参数args，它是一个字典类型，默认值为ARGS。函数通过遍历args字典中的键值对，根据键的不同来设置不同的配置值。具体的配置项如下：

- 如果args中包含"model"键，则将args['model']的值赋给self.request.default_model。
- 如果args中包含"record_dir"键，则将args['record_dir']的值赋给self.recorder.record_dir。
- 如果args中包含"max_chain_length"键，则将args['max_chain_length']的值赋给self.execution.max_chain_length。
- 如果args中包含"enable_ask_human_for_help"键，则将args['enable_ask_human_for_help']的值赋给self.execution.enable_ask_human_for_help。
- 如果args中包含"max_plan_refine_chain_length"键，则将args['max_plan_refine_chain_length']的值赋给self.execution.max_plan_refine_chain_length。
- 如果args中包含"max_plan_tree_depth"键，则将args['max_plan_tree_depth']的值赋给self.execution.max_plan_tree_depth。
- 如果args中包含"max_plan_tree_width"键，则将args['max_plan_tree_width']的值赋给self.execution.max_plan_tree_width。
- 如果args中包含"max_retry_times"键，则将args['max_retry_times']的值赋给self.max_retry_times。
- 如果args中包含"enable_self_evolve_consolidation"键，则将args['enable_self_evolve_consolidation']的值同时赋给self.execution.evolve.enable_online_plan_consolidate、self.execution.evolve.enable_plan_save、self.execution.evolve.enable_online_inner_consolidate和self.execution.evolve.enable_inner_save。
- 如果args中包含"enable_self_evolve_execution"键，则将args['enable_self_evolve_execution']的值同时赋给self.execution.evolve.enable_plan_retrieve和self.execution.evolve.enable_inner_retrieve。
- 如果args中包含"node_id"键，则判断self.toolserver.cookies是否为None，如果是，则将self.toolserver.cookies初始化为空字典，然后将args['node_id']的值赋给self.toolserver.cookies["node_id"]。

**注意**：在使用此函数时，需要传入一个字典类型的参数args，用于设置配置值。根据不同的键，可以设置不同的配置项。
## FunctionDef get_model_name
**get_model_name函数**: 这个函数的作用是根据给定的输入模型名称获取标准化的模型名称。

该函数接受一个可选的model_name参数作为输入模型名称，默认值为None。它会将输入模型名称标准化，并返回标准化后的模型名称。

如果model_name参数为None，则会使用self.request.default_model作为默认模型名称。

如果输入的模型名称无法识别，则会抛出异常。

**注意**: 使用该代码时需要注意以下几点：
- model_name参数可以为None，但如果为其他值，则必须是预定义的模型名称之一。
- 如果输入的模型名称无法识别，则会抛出异常，需要进行异常处理。

**输出示例**:
```
输入: model_name = "gpt-4-turbo"
输出: "gpt-4-turbo"

输入: model_name = "gpt-4-vision"
输出: "gpt-4-vision"

输入: model_name = "gpt-4v"
输出: "gpt-4-vision"

输入: model_name = "gpt-4"
输出: "gpt-4"

输入: model_name = "gpt-4-32k"
输出: "gpt-4-32k"

输入: model_name = "gpt-3.5-turbo-16k"
输出: "gpt-3.5-turbo-16k"

输入: model_name = "gpt4"
输出: "gpt-4"

输入: model_name = "gpt4-32"
输出: "gpt-4-32k"

输入: model_name = "gpt-35-16k"
输出: "gpt-3.5-turbo-16k"

输入: model_name = "xagentllm"
输出: "xagentllm"

输入: model_name = "unknown"
输出: "unknown"
```
## FunctionDef get_apiconfig_by_model
**get_apiconfig_by_model函数**: 此函数的功能是根据模型名称获取API配置。如果找不到给定的模型名称，则返回默认模型。

该函数首先对模型名称进行规范化处理，然后从CONFIG中获取该模型的API密钥并进行轮换。

参数:
- model_name (str): 模型的名称。

返回值:
- dict: 包含获取的API配置的字典。

该函数在以下文件中被调用：
- XAgent/ai_functions/function_manager.py的execute函数中调用了get_apiconfig_by_model函数。
- XAgent/request/obj_generator.py的client函数中调用了get_apiconfig_by_model函数。
- XAgent/request/openai.py的chatcompletion_request函数中调用了get_apiconfig_by_model函数。
- XAgent/request/xagent.py的chatcompletion_request函数中调用了get_apiconfig_by_model函数。

**注意**:
- 在调用该函数之前，需要确保已经加载了相应的API配置。
- 如果给定的模型名称在API配置中不存在，则会使用默认模型。

**输出示例**:
```
{
    "api_key": "xxxxxxxxxxxx",
    "base_url": "https://api.example.com",
    "organization": "example"
}
```
***
