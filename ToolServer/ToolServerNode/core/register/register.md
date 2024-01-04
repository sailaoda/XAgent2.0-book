# FunctionDef get_func_name
**get_func_name函数**：此函数的功能是根据给定的函数和环境返回函数的名称。

该函数接受两个参数：func和env。func是一个可调用的函数对象，env是一个BaseEnv对象，默认为None。函数根据不同的情况返回不同的函数名称。

如果env为None或者env没有env_labels属性，函数会检查func是否有tool_labels属性，并且tool_labels属性是ToolLabels类型的。如果满足条件，函数将返回func.tool_labels.name作为函数的名称；否则，返回func.__name__作为函数的名称。

如果env不为None并且env有env_labels属性，函数会检查func是否有tool_labels属性，并且tool_labels属性是ToolLabels类型的。如果满足条件，函数将返回env.env_labels.alias + '_0_' + func.tool_labels.name作为函数的名称；否则，返回env.env_labels.alias + '_0_' + func.__name__作为函数的名称。

该函数在以下文件中被调用：
文件路径：ToolServer/ToolServerNode/core/register/register.py
调用代码如下：
    def check_and_register(self, attr: Any):
        if hasattr(attr, 'tool_labels') and isinstance(attr.tool_labels, ToolLabels):
            tool_name = get_func_name(attr)
            if tool_name in self.tools:
                logger.warning(
                    f'Tool {tool_name} is replicated! The new one will be replaced!')
                return None

            self.tools[tool_name] = attr
            logger.info(f'Register tool {tool_name}!')
            return attr

        if hasattr(attr, 'env_labels') and isinstance(attr.env_labels, EnvLabels):
            # attr is a cls, need get instance
            if attr.env_labels.name in self.envs:
                return
            if not issubclass(attr, BaseEnv):
                raise Exception(
                    f'The env {attr.env_labels.name} is not a subclass of BaseEnv!')
            env = attr(config=self.config.dict())
            env_tools = {}

            if self.toolregister_cfg['parent_tools_visible']:
                func_names = env.__get_all_func_name__()
            else:
                func_names = env.__get_defined_func_name__()

            for func_name in func_names:
                func = getattr(env, func_name)
                if hasattr(func, 'tool_labels'):
                    env_tools[get_func_name(func, env)] = func

            env_keys = set(env_tools.keys())
            tools_keys = set(self.tools.keys())
            if env_keys & tools_keys:
                logger.warning(
                    f'Env {env.env_labels.name} has tools with same name as other tools! The new one will be ignored!')
                for tool_name in env_keys & tools_keys:
                    env_tools.pop(tool_name)

            # disable env tools in global tools
            # self.tools.update(env_tools)
            self.envs_cls[attr.env_labels.name] = attr
            self.envs[attr.env_labels.name] = [env]
            logger.info(
                f'Register env {env.env_labels.name} with {len(env_tools)} tools!')

            return env

        return None

[此部分代码结束]
文件路径：ToolServer/ToolServerNode/extensions/envs/shell.py
调用代码如下：
    def read_stdout(self, probe: bool = False) -> str:
        """读取shell的stdout流。如果stderr不为空，则返回stderr。
        
        如果stdout和stderr都为空，则返回空字符串。
        您可以使用此函数检查shell是否有新内容需要读取，以便运行时间较长的进程。
        
        :param boolean? probe: 如果为`True`，则如果没有输出准备好，函数将立即返回；否则，它将引发`OutputNotReady`异常，并要求调用`next_calling`中的函数来获取结果。
        """
        if not self.running:
            raise RuntimeError('Shell is not running!')
        
        ready_fds,_,_ = select.select(self.output_fileno,[],[],0.01)
        if probe and len(ready_fds) == 0 :
            raise OutputNotReady('Output is not ready!',next_calling=get_func_name(self.read_stdout,self),arguments={'probe':True})
        
        error = read_pipe(self.running_proc.stderr)
        if error:
            return error

        return read_pipe(self.running_proc.stdout)

[此部分代码结束]
调用代码如下：
    def write_stdin(self, content:str) -> str:
        """向shell的stdin流写入内容，并从stderr或stdout获得即时反馈。
        
        示例：
        ```
        write_stdin('echo "hello world"')
        ```
        这将在shell中执行命令`echo "hello world"`并返回输出`hello world`。
        
        :param string content: 要写入的内容。
        """

        # 暂时删除，可能稍后放回？
        # 您可能需要调用`read_stdout`来获取运行时间较长的进程的进一步反馈。
        if not self.running:
            raise RuntimeError('Shell is not running!')
        if not content.endswith("\n"):
            content += "\n"
        self.running_proc.stdin.write(content)
        self.running_proc.stdin.flush()
        
        ready_fds,_,_ = select.select(self.output_fileno,[],[],0.01)
        if len(ready_fds) == 0:
            raise OutputNotReady('Output is not ready!',next_calling=get_func_name(self.read_stdout,self),arguments={'probe':True})
        
        return 'Instant shell output: ' + self.read_stdout()

[此部分代码结束]

**注意**：使用此代码的注意事项
**输出示例**：模拟代码返回值的可能外观。

请注意：
- 生成的内容中不应包含Markdown的标题和分隔符语法。
- 主要使用目标语言编写。如果需要，可以在分析和描述中使用一些英文单词，以增强文档的可读性，因为您不需要将函数名或变量名翻译成目标语言。
***
# ClassDef ToolRegister
**ToolRegister函数**：ToolRegister类的功能是管理工具和环境的注册和加载。

ToolRegister类是一个工具注册器，用于管理工具和环境的注册和加载。它具有以下功能：

- 初始化函数：初始化ToolRegister对象，并根据传入的配置参数进行配置。
- 注册工具：将工具注册到工具注册器中，并将工具的代码保存到文件中。
- 注册环境：将环境注册到工具注册器中，并将环境的实例保存到环境列表中。
- 创建新环境实例：根据环境名称创建新的环境实例，并将其添加到环境列表中。
- 销毁环境实例：根据环境名称和环境索引销毁指定的环境实例。
- 获取所有环境：返回所有环境的描述信息。
- 获取所有环境工具：返回所有环境中的工具的绝对名称。
- 获取所有环境工具字典：返回所有环境中的工具的描述信息。
- 获取工具字典：根据工具名称返回工具的描述信息。
- 获取环境字典：根据环境名称和环境索引返回环境的描述信息。
- 获取所有工具：返回所有可用的工具的名称。
- 获取所有工具字典：返回所有可用的工具的描述信息。
- 动态加载扩展：动态加载扩展模块，并将其中的工具和环境注册到工具注册器中。

注意：在使用ToolRegister类时，需要注意以下几点：
- 在注册工具时，需要确保工具的名称唯一，并且工具的代码已经包装在`@toolwrapper()`装饰器中。
- 在注册环境时，需要确保环境的名称唯一，并且环境是BaseEnv类的子类。
- 在动态加载扩展时，需要确保扩展模块中的工具和环境已经正确注册。

**输出示例**：
```python
{
    "available_tools": ["tool1", "tool2", "env1_tool1", "env1_tool2"],
    "tools_json": [
        {
            "name": "tool1",
            "description": "This is tool1",
            ...
        },
        {
            "name": "tool2",
            "description": "This is tool2",
            ...
        },
        {
            "name": "env1_tool1",
            "description": "This is env1_tool1",
            ...
        },
        {
            "name": "env1_tool2",
            "description": "This is env1_tool2",
            ...
        }
    ]
}
```
## FunctionDef __init__
**__init__函数**: 该函数的功能是初始化ToolRegister类的实例。

该函数首先接受一个可选的字典参数`config`，默认为空字典。在函数体内，首先会深拷贝一个全局配置常量`CONFIG`到实例变量`self.config`中。然后，遍历传入的`config`字典，将其键值对更新到`self.config`中，这样可以根据传入的配置定制化实例的配置。

接下来，函数从`self.config`中提取`toolregister`配置，并将其存储在实例变量`self.toolregister_cfg`中。同时，初始化`self.tool_creation_context`字典和`self.tool_creation_context_load_code`列表，用于存储工具创建上下文和加载代码。

函数遍历`self.toolregister_cfg`中的`tool_creation_context`配置，动态加载指定的模块和类，并将它们存储在`self.tool_creation_context`中。加载代码字符串被添加到`self.tool_creation_context_load_code`列表中。

此外，函数还初始化了三个字典：`self.tools`用于存储工具函数，`self.envs`用于存储环境实例列表，`self.envs_cls`用于存储环境类的构造函数。

函数通过`importlib`动态加载`core.envs`和`core.tools`模块中的所有子模块，并通过`self.check_and_register`方法检查并注册这些模块中的属性。

最后，如果`self.config`中存在`enabled_extensions`配置，并且其为列表类型，则遍历该列表，动态加载每个扩展模块。

在加载完所有工具和环境后，使用`logger.info`打印加载的工具和环境的数量。

**注意**:
- 在使用`exec`和`eval`执行动态代码时需要格外小心，因为这可能会引入安全风险。确保传入的代码是可信的。
- 动态加载模块和类时，需要确保模块和类的路径正确无误。
- 该初始化函数依赖于全局配置常量`CONFIG`，因此在使用前需要确保`CONFIG`已经被正确定义和初始化。
- 在实际部署中，应当注意日志记录的级别和信息的详细程度，以避免在生产环境中输出过多的调试信息。
## FunctionDef check_and_register
**check_and_register函数**: 该函数的作用是检查并注册工具或环境。

该函数`check_and_register`是ToolServerNode项目中用于注册工具（tools）和环境（envs）的核心方法。它接受一个参数`attr`，这个参数可以是一个工具函数或者是一个环境类。函数的主要逻辑分为两部分：

1. 如果`attr`具有`tool_labels`属性，并且该属性是`ToolLabels`类的实例，那么该函数会尝试将`attr`注册为一个工具。它首先通过`get_func_name`函数获取工具的名称，然后检查该名称是否已经在工具注册表`self.tools`中。如果是，会记录一条警告日志，并返回`None`。如果不是，会将工具添加到注册表中，并记录一条信息日志。

2. 如果`attr`具有`env_labels`属性，并且该属性是`EnvLabels`类的实例，那么该函数会尝试将`attr`注册为一个环境。首先，它会检查环境名称是否已经在环境注册表`self.envs`中。如果已经存在，则直接返回。接着，它会检查`attr`是否是`BaseEnv`的子类，如果不是，则抛出异常。然后，它会创建环境的实例，并获取环境中定义的所有工具函数。如果配置允许，它还会获取父环境中定义的工具函数。最后，它会将环境和其工具注册到相应的注册表中，并记录一条信息日志。

在项目中，`check_and_register`函数被调用于几个不同的场景：

- 在`ToolServerNode/core/register/register.py`的`__init__`方法中，它被用于加载并注册`core.envs`和`core.tools`模块中定义的所有工具和环境。
- 在同一文件中的`register_tool`方法中，它被用于注册用户动态创建的工具。
- 在`dynamic_extension_load`方法中，它被用于动态加载并注册扩展模块中的工具和环境。

**注意**：
- 使用该函数时，需要确保传入的`attr`参数具有正确的`tool_labels`或`env_labels`属性。
- 如果工具或环境的名称已存在于注册表中，工具将不会被注册，并且会记录一条警告日志。
- 环境必须是`BaseEnv`的子类，否则会抛出异常。

**输出示例**：
- 如果成功注册了一个工具，可能会记录如下信息日志：
  ```
  Register tool example_tool!
  ```
- 如果尝试注册一个已存在的工具，可能会记录如下警告日志：
  ```
  Tool example_tool is replicated! The new one will be replaced!
  ```
- 如果成功注册了一个环境，并且该环境包含了两个工具，可能会记录如下信息日志：
  ```
  Register env example_env with 2 tools!
  ```
## FunctionDef create_new_env_instance
**create_new_env_instance 函数**: 此函数的功能是创建一个新的环境实例并将其添加到环境字典中。

此函数`create_new_env_instance`是一个用于创建并初始化一个新的环境实例的方法。它接受两个参数：`env_name`和`**kwargs`。`env_name`是一个字符串，表示要创建的环境的名称。`**kwargs`是一个可变关键字参数，用于传递给环境类构造函数的参数。

函数的工作流程如下：
1. 通过`env_name`在`self.envs_cls`字典中查找对应的环境类。
2. 使用`**kwargs`中的参数创建该环境类的一个新实例。
3. 将新创建的环境实例添加到`self.envs`字典中`env_name`对应的列表中。
4. 返回新创建的环境实例。

**注意**：
- 确保`env_name`在`self.envs_cls`中有对应的环境类，否则会引发`KeyError`。
- 传递给`**kwargs`的参数应该与环境类的构造函数参数匹配，否则可能会引发`TypeError`。

**输出示例**：
假设存在一个名为`"my_env"`的环境类，并且该类的构造函数接受一个名为`config`的参数。调用`create_new_env_instance("my_env", config=my_config)`可能会返回如下实例：
```python
<my_env_instance: my_env(config=my_config)>
```
## FunctionDef destory_env_instance
**destory_env_instance 函数**: 此函数的功能是销毁指定环境实例。

`destory_env_instance` 函数是一个用于销毁特定环境实例的方法。它接受两个参数：`env_name` 和 `env_idx`。`env_name` 参数指定了要销毁的环境的名称，而 `env_idx` 参数指定了在环境列表中的索引。

函数的执行流程如下：
1. 通过 `env_name` 从 `self.envs` 字典中获取到对应的环境列表。
2. 使用 `pop` 方法从该列表中移除索引为 `env_idx` 的环境实例。
3. 使用 `del` 语句彻底删除该环境实例的引用，确保其被垃圾回收器回收。

此函数的主要作用是管理环境实例的生命周期，当一个环境实例不再需要时，可以通过此函数来安全地销毁它，释放相关资源。

**注意**：
- 在调用此函数之前，确保 `env_name` 存在于 `self.envs` 字典中，且 `env_idx` 是一个有效的索引，否则可能会引发 `IndexError` 或 `KeyError`。
- 销毁环境实例后，应当确保不再有任何引用指向该实例，避免出现悬挂引用。
- 由于此函数会修改 `self.envs` 字典，调用时需要注意线程安全问题，避免在多线程环境下产生竞态条件。
## FunctionDef get_all_env_tools
**get_all_env_tools 函数**: 此函数的功能是列出所有环境中的工具，并返回它们的绝对名称列表。

此函数`get_all_env_tools`属于某个类（尽管代码片段中没有明确显示类定义），它的作用是遍历该类中的`envs`属性，该属性包含了多个环境对象。对于每个环境对象，函数会进一步遍历其`env_labels.subtools_labels`属性，这个属性包含了该环境下所有工具的名称。函数将每个工具的名称与其所在环境的别名和索引号结合起来，形成一个包含环境别名、环境索引和工具名称的字符串，并将这些字符串添加到结果列表`all_env_tools`中。

具体来说，函数执行以下步骤：
1. 初始化一个空列表`all_env_tools`，用于存储所有工具的绝对名称。
2. 遍历`self.envs`字典中的每个环境名称`env_name`。
3. 对于每个环境名称，再遍历其对应的环境对象列表，获取每个环境对象`env`及其索引`env_idx`。
4. 对于每个环境对象，遍历其`env_labels.subtools_labels`属性中的每个工具名称`tool_name`。
5. 将环境的别名`env.env_labels.alias`、环境索引`env_idx`和工具名称`tool_name`按照格式`alias_idx_toolname`组合成字符串，并添加到`all_env_tools`列表中。
6. 返回包含所有工具绝对名称的列表`all_env_tools`。

在项目中，`get_all_env_tools`函数被`ToolServer/ToolServerNode/main.py`文件中的`get_available_tools`异步函数调用。`get_available_tools`函数的目的是获取并返回可用的工具和环境，它通过调用`tool_register`对象的`get_all_tools`、`get_all_env_tools`以及`get_all_tools_dict`和`get_all_env_tools_dict`方法来实现。这表明`get_all_env_tools`函数提供的信息是用于构建表示可用工具和环境的响应数据的一部分。

**注意**：
- 在调用此函数之前，确保`self.envs`已经被正确初始化并填充了环境对象。
- 函数返回的列表中的字符串格式为`环境别名_环境索引_工具名称`，调用者应当了解这一格式以便正确解析和使用返回的数据。

**输出示例**：
假设有两个环境，别名分别为`envA`和`envB`，且`envA`中有一个工具`tool1`，`envB`中有两个工具`tool2`和`tool3`，则函数可能返回以下列表：
```python
["envA_0_tool1", "envB_0_tool2", "envB_1_tool3"]
```
## FunctionDef get_all_env_tools_dict
**get_all_env_tools_dict函数**: 该函数的功能是列出所有环境中的工具，并提供包含工具描述的字典列表。

该函数`get_all_env_tools_dict`属于某个类（在代码中未显示该类的名称，但可以推测它可能是`ToolRegister`或类似的注册管理类），用于获取所有环境（envs）中注册的工具，并将它们的描述以字典列表的形式返回。这个列表中的每个字典都包含了一个工具的详细信息。

具体来说，函数执行以下步骤：
1. 初始化一个空列表`all_env_tools`，用于存放所有工具的描述。
2. 遍历`self.envs`字典中的每个环境名称`env_name`。
3. 对于每个环境名称，遍历其索引`env_idx`和对应的环境实例`env`。
4. 在每个环境实例中，遍历其环境标签`env.env_labels`中的子工具标签`subtools_labels`。
5. 对于每个子工具，获取其描述并转换为字典形式`tool_des`。
6. 更新`tool_des`字典，添加一个新的键值对，键为`"name"`，值为环境别名、环境索引和工具名称的组合，格式为`env.env_labels.alias+'_'+str(env_idx)+'_'+tool_name`。
7. 将更新后的`tool_des`字典添加到`all_env_tools`列表中。
8. 函数返回填充完成的`all_env_tools`列表。

在项目中，`get_all_env_tools_dict`函数被`ToolServer/ToolServerNode/main.py`文件中的`get_available_tools`异步函数调用。`get_available_tools`函数用于返回注册在`ToolRegister`中的可用工具和环境，以及这些工具的JSON表示。在返回的字典中，`"tools_json"`键对应的值是通过调用`get_all_tools_dict`和`get_all_env_tools_dict`函数获取的所有工具描述的列表。

**注意**：
- 在使用`get_all_env_tools_dict`函数时，需要确保`self.envs`已经被正确初始化，并且每个环境实例都有一个有效的`env_labels`属性，该属性中包含了`subtools_labels`字典。
- 该函数返回的列表中包含的字典，其键值对应于工具的描述信息，这可能包括工具的名称、版本、功能描述等，具体取决于`env.env_labels.subtools_labels[tool_name].dict()`的实现。

**输出示例**：
```python
[
    {
        "name": "环境别名_0_工具名称",
        "version": "1.0.0",
        "description": "工具的功能描述",
        # ... 其他工具描述信息
    },
    # ... 更多工具描述字典
]
```
在这个示例中，每个字典代表一个工具的描述，`"name"`键对应的值是由环境别名、环境索引和工具名称组合而成的唯一标识符。其他键如`"version"`和`"description"`则提供了工具的版本和功能描述。
## FunctionDef register_tool
**register_tool函数**: 该函数的作用是注册一个新的工具。

该`register_tool`函数是`ToolServerNode`项目中的一个核心功能，它允许用户通过提供工具名称和代码来注册一个新的工具。该函数接收两个参数：`tool_name`和`code`。`tool_name`是一个字符串，表示要注册的工具的名称；`code`是一个字符串，包含了工具的代码。

函数的执行流程如下：
1. 首先，函数尝试在一个安全的上下文中执行传入的`code`字符串。这是通过`exec`函数实现的，它在`self.tool_creation_context`这个字典上下文中执行代码。如果在执行过程中发生异常，函数会捕获这个异常，并使用`traceback.format_exc()`获取异常的详细信息，然后记录到日志中，并抛出一个`ToolRegisterError`异常，指明执行新工具代码失败。

2. 然后，函数尝试从上下文中通过`eval`函数获取工具函数。如果`tool_name`在上下文中不存在，将抛出`ToolRegisterError`异常，提示无法找到工具。

3. 接下来，函数调用`self.check_and_register`方法来检查并注册工具函数。如果工具函数没有标签或者是重复的，`check_and_register`方法将返回`None`，此时函数将抛出`ToolRegisterError`异常，提示工具没有标签或者重复，并确保使用`@toolwrapper()`装饰器包装工具。

4. 最后，函数将工具的代码写入到`extensions/tools`目录下对应的`tool_name.py`文件中，并返回一个包含工具信息的字典。

在项目中的调用情况如下：
在`ToolServer/ToolServerNode/main.py`文件中，定义了一个异步函数`register_new_tool`，它通过HTTP请求体接收`tool_name`和`code`参数，并调用`register_tool`函数来注册新工具。如果在注册过程中发生异常，它会捕获异常并记录，然后抛出一个`HTTPException`异常，指明在注册新工具时发生错误。

**注意**：
- 在使用`exec`执行代码时，需要确保代码是安全的，以防止执行恶意代码。
- 工具代码需要正确地定义，并且能够在给定的上下文中执行。
- 工具名称需要唯一，不能与已注册的工具重名。
- 工具函数需要使用`@toolwrapper()`装饰器进行包装，以便正确注册。

**输出示例**：
```python
{
    'tool_name': 'example_tool',
    'description': 'This is an example tool.',
    'labels': ['example', 'demo']
}
```
这是一个假设的返回值示例，其中包含了工具的名称、描述和标签。实际的返回值将取决于注册工具时提供的信息。
## FunctionDef dynamic_extension_load
**dynamic_extension_load函数**: 此函数的功能是动态加载扩展模块。

`dynamic_extension_load` 函数是一个用于动态加载指定路径下的扩展模块并注册其内容的方法。它接受一个字符串参数 `extension`，该参数是要加载的模块的路径。如果加载成功，函数返回 `True`；如果在加载过程中发生任何异常，它将记录错误信息并返回 `False`。

详细代码分析如下：
1. 函数首先尝试使用 `importlib.import_module` 方法根据提供的模块路径动态导入模块。
2. 导入模块后，函数遍历模块中的所有属性，对于每个属性，它调用 `check_and_register` 方法进行检查和注册。这通常涉及到检查属性是否是某种特定类型的工具或环境，并将其添加到相应的注册表中。
3. 如果在导入或注册过程中发生异常，函数将捕获异常并使用 `logger.error` 记录错误信息，然后返回 `False` 表示加载失败。
4. 如果没有异常发生，函数将返回 `True` 表示加载成功。

在项目中调用情况分析：
- 在 `ToolServer/ToolServerNode/core/register/register.py` 文件的 `__init__` 方法中，`dynamic_extension_load` 被用来加载配置中指定的启用扩展。
- 在 `__getitem__` 方法中，当尝试获取一个未注册的工具或环境时，`dynamic_extension_load` 会被用来尝试动态加载对应的扩展模块，如果加载成功并且所需的工具或环境存在于扩展中，它将被返回。

**注意**：
- 使用此函数时，需要确保提供的模块路径是正确的，并且模块本身没有错误，否则会导致加载失败。
- 动态加载模块可能会带来一定的性能开销，因此应当谨慎使用，避免在频繁调用的场景中过度使用。

**输出示例**：
如果加载成功，函数将返回：
```python
True
```
如果加载失败，函数将返回：
```python
False
```
## FunctionDef get_tool_dict
**get_tool_dict函数**: 此函数的功能是获取指定工具名称的工具字典。

`get_tool_dict`函数定义在`ToolServerNode`的`core/register/register.py`文件中，它是`ToolRegister`类的一个方法。该方法接受一个工具名称作为参数，返回与该工具名称对应的工具标签字典。

具体来说，该方法通过工具名称作为键从`ToolRegister`对象（即`self`）中检索对应的工具对象。然后，它调用该工具对象的`tool_labels`属性的`dict`方法，生成并返回一个包含工具标签信息的字典。`name_overwrite`参数用于在生成的字典中重写工具名称。

在项目中，`get_tool_dict`函数被用于两个场景：
1. 在工具注册流程中，当一个新的工具通过`register_tool`方法注册后，`get_tool_dict`被用来获取新注册工具的标签字典，以便将其写入文件或进行其他处理。
2. 在`ToolServerNode/main.py`文件中，`get_json_schema_for_tool`函数使用`get_tool_dict`来为每个请求的工具名称生成JSON schema。这通常用于前端应用，以便为用户提供工具的结构化信息。

**注意**：
- 在调用`get_tool_dict`方法之前，确保工具已经被正确注册到`ToolRegister`中，否则会导致键错误。
- 如果工具名称不存在于`ToolRegister`中，那么在尝试获取其标签字典时应当处理可能出现的异常。

**输出示例**：
如果工具名称为`example_tool`，并且该工具具有标签`version: 1.0`和`description: 示例工具`，那么`get_tool_dict`的返回值可能如下所示：
```python
{
    'name': 'example_tool',
    'version': '1.0',
    'description': '示例工具'
}
```
这个字典包含了工具的名称、版本和描述等信息，具体内容取决于工具的标签定义。
## FunctionDef get_env_dict
**get_env_dict函数**: 该函数的功能是获取指定环境名称和索引对应的环境字典。

该`get_env_dict`函数是`ToolServerNode`项目中`ToolRegister`类的一个方法，它的作用是根据提供的环境名称(`env_name`)和环境索引(`env_idx`)来获取相应的环境配置字典。这个方法主要用于获取特定工具环境的配置信息，以便其他部分的代码可以使用这些信息来执行相应的操作。

具体来说，该函数首先检查传入的环境名称是否存在于`self.envs`字典中。如果不存在，会抛出一个`EnvNotFound`异常，提示环境名称未找到。如果环境名称存在，但是提供的索引`env_idx`超出了该环境的索引范围，也会抛出`EnvNotFound`异常，并提示具体的环境索引未找到。

如果环境名称和索引都有效，函数将返回`self.envs[env_name][env_idx]`中的`env_labels`属性的字典形式。`env_labels`是一个包含环境标签信息的对象，`dict()`方法将其转换为字典，并且通过`include_invisible=True`和`max_show_tools=-1`参数来包含所有可能的环境标签，无论它们是否可见，以及不限制显示的工具数量。

在项目中，`get_env_dict`函数被`ToolServer/ToolServerNode/main.py`文件中的`get_json_schema_for_env`异步函数调用。`get_json_schema_for_env`函数接收一个环境名称列表，遍历这个列表，检查每个环境名称是否存在于`tool_register`的`envs`中。如果存在，它会使用`get_env_dict`函数获取该环境的配置字典，并将其添加到返回的JSON中。如果环境名称不存在，则将其添加到错误名称列表中。最终，函数返回一个包含所有请求的环境的JSON配置和缺失环境名称列表的字典。

**注意**：使用`get_env_dict`函数时，需要确保传入的环境名称在`ToolRegister`的`envs`属性中存在，且索引值在有效范围内。否则，函数将抛出异常。

**输出示例**:
假设存在一个名为"python_env"的环境配置，其索引为0，那么函数的返回值可能看起来像这样：

```python
{
    'name': 'python_env',
    'version': '3.8',
    'path': '/usr/bin/python3',
    'visible': True,
    'tools': ['pip', 'pytest', 'flask']
}
```

这个字典包含了环境的名称、版本、路径等信息，以及一个包含该环境中可用工具的列表。
## FunctionDef get_all_envs
**get_all_envs函数**：该函数的功能是获取所有环境的信息。

该函数首先创建一个空列表envs，用于存储所有的环境对象。然后通过遍历self.envs中的每个环境名称，将该环境下的所有环境对象添加到envs列表中。最后，将envs列表中每个环境对象的env_labels属性转换为字典形式，并返回包含所有环境信息的列表。

**注意**：在使用该函数时需要注意以下几点：
- 该函数需要在ToolRegister对象中调用，因此在调用之前需要先创建ToolRegister对象。
- 该函数返回的是一个包含所有环境信息的列表，每个环境信息以字典形式表示。

**输出示例**：以下是该函数可能返回值的示例：
```
[
    {
        "name": "env1",
        "alias": "环境1",
        "description": "这是环境1的描述",
        ...
    },
    {
        "name": "env2",
        "alias": "环境2",
        "description": "这是环境2的描述",
        ...
    },
    ...
]
```
## FunctionDef get_all_tools
**get_all_tools函数**: 该函数的功能是获取所有注册的工具名称列表。

该函数`get_all_tools`定义在`ToolServerNode/core/register/register.py`文件中，属于一个注册管理类的成员方法。此函数的主要作用是从注册表中检索并返回所有已注册的工具名称。它接受一个布尔参数`include_invisible`，用于控制是否包含那些被标记为不可见的工具。

详细代码分析如下：
- 函数定义了一个参数`include_invisible`，默认值为`False`。这意味着在默认情况下，函数只返回那些被标记为可见的工具名称。
- 如果`include_invisible`为`True`，则函数会返回注册表中所有工具的名称，不论其可见性如何。
- 如果`include_invisible`为`False`，则函数通过列表推导式遍历`self.tools`字典，并且只选择那些其`tool_labels.visible`属性为`True`的工具名称返回。这里`self.tools`是一个字典，其键为工具名称，值为工具的实例对象。
- 返回值是一个字符串列表，包含了符合条件的工具名称。

在项目中的调用情况：
- 在同一文件`register.py`中，有一个名为`get_all_tools_dict`的函数调用了`get_all_tools`函数。这个函数返回一个包含所有工具标签字典的列表，其中可以选择是否包含不可见的工具。
- 在`ToolServer/ToolServerNode/main.py`文件中的`get_available_tools`异步函数中，调用了`get_all_tools`函数来获取所有可用的工具名称，并将其与环境工具名称合并后返回。

**注意**：
- 在使用`get_all_tools`函数时，需要注意其返回的是工具名称的列表，而不是工具对象。
- 如果需要获取工具的详细信息或其他属性，可能需要通过工具名称进一步访问工具注册表中的相应工具实例。

**输出示例**：
如果注册表中有三个工具，分别命名为`tool1`、`tool2`和`tool3`，其中`tool2`被标记为不可见，那么调用`get_all_tools()`将返回：
```python
['tool1', 'tool3']
```
而调用`get_all_tools(include_invisible=True)`将返回：
```python
['tool1', 'tool2', 'tool3']
```
## FunctionDef get_all_tools_dict
**get_all_tools_dict函数**: 该函数的功能是获取所有工具的字典列表。

该函数`get_all_tools_dict`属于某个类（可能是`ToolRegister`或者类似的工具注册管理类），用于返回一个包含所有注册工具的字典列表。每个工具都通过其名称作为键，并且每个工具的详细信息以字典形式表示。

具体来说，该函数接受一个参数`include_invisible`，默认值为`False`。当`include_invisible`为`True`时，函数将返回包括不可见工具在内的所有工具的列表；当为`False`时，只返回可见工具的列表。

函数内部，首先调用`self.get_all_tools(include_invisible)`方法获取所有工具的名称列表，然后对于每个工具名称，通过`self.tools[tool_name].tool_labels.dict(name_overwrite=tool_name)`获取工具的标签信息，并将其转换为字典格式。这里`tool_labels`很可能是一个具有序列化能力的对象，能够将工具的标签信息转换为字典格式，并且可以通过`name_overwrite`参数重写工具名称。

在项目中，`get_all_tools_dict`函数被`ToolServer/ToolServerNode/main.py`文件中的`get_available_tools`异步函数调用。`get_available_tools`函数用于获取并返回所有可用的工具和环境，以及它们的JSON表示。在这个上下文中，`get_all_tools_dict`函数的返回值被用来构建返回给客户端的`tools_json`字段，这个字段包含了所有工具的JSON表示。

**注意**：
- 在调用`get_all_tools_dict`函数时，需要确保`self.tools`已经被正确初始化，并且包含了所有需要的工具对象。
- 如果需要获取包括不可见工具在内的所有工具，需要将`include_invisible`参数设置为`True`。

**输出示例**：
```python
[
    {
        "name": "tool1",
        "version": "1.0",
        "description": "This is a description of tool1.",
        "visible": True
    },
    {
        "name": "tool2",
        "version": "2.0",
        "description": "This is a description of tool2.",
        "visible": False
    }
]
```
在这个示例中，返回的列表包含了两个工具的字典表示，每个字典包含了工具的名称、版本、描述和可见性等信息。
## FunctionDef __getitem__
**__getitem__函数**: 该函数的功能是根据提供的键（key）检索并返回相应的可调用对象（Callable）。

该函数是一个特殊方法，定义在一个注册表类中，用于通过键来检索注册表中的工具或环境下的子工具。该方法支持两种类型的键：字符串和元组。

当键是字符串时，该方法首先检查该字符串是否直接对应于注册表中的某个工具。如果不是，它会尝试将字符串分割并解析为环境名和工具名，然后再次尝试检索。如果工具在注册表中找不到，它会尝试动态加载扩展中的工具。如果仍然找不到，将抛出`ToolNotFound`异常。

当键是元组时，该方法会根据元组的长度来解析环境名、环境索引和工具名。如果元组长度为2，假定环境索引为0；如果长度为3，则分别解析出环境名、环境索引和工具名。如果环境索引超出范围或环境名在注册表中不存在，将尝试动态加载扩展中的环境。如果环境中不存在指定的工具，将抛出`ToolNotFound`异常。如果找到了相应的工具，将返回该工具对应的函数。

如果键的类型不是字符串或元组，或者元组的长度不是2或3，将抛出`NotImplementedError`异常。

**注意**：
- 使用该方法时，需要确保传入的键是正确的格式，否则可能会抛出异常。
- 动态加载扩展的功能需要`dynamic_extension_load`方法的支持，该方法需要在类中实现。
- 异常`ToolNotFound`和`EnvNotFound`需要在类中或全局范围内定义。

**输出示例**：
假设注册表中有一个环境名为"web"，索引为0，且该环境下有一个名为"search"的工具，那么：
```python
register = Register()
func = register["web", 0, "search"]
```
上述代码将返回环境"web"中索引为0的子工具"search"对应的函数。如果该工具不存在，则会抛出`ToolNotFound`异常。
***
