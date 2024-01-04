# FunctionDef add_running_support_for_raw_code
**add_running_support_for_raw_code函数**: 此函数的功能是为原始代码添加异步执行和包装器语法支持。

该函数`add_running_support_for_raw_code`接收一个字符串参数`raw_code`，该字符串代表一段Python代码。函数的主要目的是修改这段代码，使其能够支持异步执行，并且将特定的函数调用包装在DSL（领域特定语言）框架的上下文中。

函数执行的步骤如下：

1. 首先，将输入的代码字符串`raw_code`按行分割成列表`code_lines`。
2. 定义一个空列表`function_list`，用于存储代码中定义的函数名。
3. 遍历`code_lines`中的每一行，寻找以`def`开头的行，这些行代表函数定义。
4. 对于每个函数定义，提取函数名，并将其添加到`function_list`中。
5. 如果函数名以`mainWorkflow`或`auto_`开头，则在该行前添加`@dsl_code_wrapper`装饰器，并在`def`前添加`async`关键字，以标记为异步函数。
6. 对于其他函数（即动作函数），在该行前添加`@dsl_action_wrapper`装饰器，并在`def`前添加`async`关键字。
7. 获取当前文件的根目录路径，并转换为Python模块路径格式，用于导入`dsl_code_wrapper`和`dsl_action_wrapper`装饰器以及`DSLRouteResult`类。
8. 在代码的开头添加导入语句，以便可以使用这些装饰器和类。
9. 为了支持异步调用，函数遍历`function_list`中的函数名，并在代码中搜索这些函数的调用。如果找到函数调用，就在调用前添加`await`关键字，以实现异步调用。
10. 最后，将修改后的代码行列表`code_lines`合并成一个字符串，并返回。

**注意**：
- 在使用此函数时，需要确保传入的`raw_code`是有效的Python代码。
- 该函数假设所有的函数调用都需要变为异步调用，因此在实际使用中可能需要根据具体情况调整。
- 函数中硬编码了一些特殊的函数名（如`AI_route_and_give_params_`和`AI_complete_params_`系列），这些可能与特定DSL框架的实现细节相关。

**输出示例**：
假设原始代码如下：
```python
def example_function(param):
    return param

def mainWorkflow():
    result = example_function("Hello")
    return result
```
调用`add_running_support_for_raw_code`函数后，返回的代码可能会变为：
```python
from XAgent.engines.dsl import dsl_code_wrapper, dsl_action_wrapper, DSLRouteResult

@dsl_code_wrapper
async def mainWorkflow():
    result = await example_function("Hello")
    return result

async def example_function(param):
    return param
```
在这个示例中，`mainWorkflow`函数被添加了`@dsl_code_wrapper`装饰器和`async`关键字，而`example_function`函数调用前面添加了`await`关键字，以支持异步调用。
***
