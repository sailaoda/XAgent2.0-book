# FunctionDef get_intrinsic_functions
**get_intrinsic_functions函数**：该函数的作用是返回内置函数的列表。

该函数返回一个包含内置函数的列表，这些函数可以在工作流中使用。列表中包括了用于定义新函数、重写函数参数、实现工作流以及向用户请求帮助的函数。每个函数都有名称、描述和参数。

返回值：
- list：内置函数的列表。

**node_define_function**：
- 名称：function_define
- 描述：定义一个函数列表，每个函数都必须指定给定的（integration-resource-action）
- 参数：
  - type：object
  - properties：
    - thought：
      - 类型：string
      - 描述：为什么选择这个函数
    - plan：
      - 类型：array
      - 描述：接下来的步骤是什么
      - items：
        - 类型：string
    - criticism：
      - 类型：string
      - 描述：当前计划的主要弱点是什么
    - functions：
      - 类型：array
      - 描述：新定义的函数
      - items：
        - 类型：object
        - properties：
          - integration_name：
            - 类型：string
          - resource_name：
            - 类型：string
          - operation_name：
            - 类型：string
          - comments：
            - 类型：string
            - 描述：这将显示给用户，你将如何在工作流中使用此节点
          - TODO：
            - 类型：array
            - 描述：该函数尚未实现。列出你将如何进一步实现、测试和完善此函数的步骤
            - items：
              - 类型：string
      - required：["integration_name", "resource_name", "operation_name", "comments", "TODO"]

**node_rewrite_param_function**：
- 名称：function_rewrite_params
- 描述：给定函数的参数和提供的参数描述，这将覆盖现有参数
- 参数：
  - type：object
  - properties：
    - thought：
      - 类型：string
      - 描述：为什么选择这个函数
    - plan：
      - 类型：array
      - 描述：接下来的步骤是什么
      - items：
        - 类型：string
    - criticism：
      - 类型：string
      - 描述：当前计划的主要弱点是什么
    - function_name：
      - 类型：string
      - 描述：例如'Action_x'或'Trigger_x'。必须是已定义的函数。
    - params：
      - 类型：string
      - 描述：输入参数的json对象。字段描述应参考代码中的函数定义。
    - comments：
      - 类型：string
      - 描述：这将显示给用户，你将如何在工作流中使用此节点
    - TODO：
      - 类型：array
      - 描述：你将如何进一步实现、测试和完善此函数
      - items：
        - 类型：string
    - required：["thought", "plan", "criticism", "function_name", "params", "comments", "TODO"]

**workflow_implement_function**：
- 名称：workflow_implment
- 描述：实现一个工作流，直接编写工作流的输出，以"def mainWorkflow..."或"def subworkflow_xxx..."开头
- 参数：
  - type：object
  - properties：
    - thought：
      - 类型：string
      - 描述：为什么选择这个函数
    - plan：
      - 类型：array
      - 描述：接下来的步骤是什么
      - items：
        - 类型：string
    - criticism：
      - 类型：string
      - 描述：当前计划的主要弱点是什么
    - workflow_name：
      - 类型：string
      - 描述：新实现的工作流。如果该工作流已存在，将进行覆盖。所有名称必须是"mainWorkflow"或"subworkflow_x"
    - code：
      - 类型：string
      - 描述：编写工作流的Python代码。我们将检查你是否有"comments"和"TODOs"。mainWorkflow的输入应始终是trigger_input，子工作流的输入应始终是father_workflow_input
    - required：["thought", "plan", "criticism", "workflow_name", "code"]

**ask_user_help_function**：
- 名称：ask_user_help
- 描述：当你认为无法解决某些问题时，调用此函数。你可以向用户寻求帮助。
- 参数：
  - type：object
  - properties：
    - problems：
      - 类型：string
      - 描述：告诉用户你面临的问题并寻求帮助。

**task_submit_function**：
- 名称：task_submit
- 描述：当你认为任务已完成时，调用此函数。用户将给出一些反馈，这些反馈可以帮助你改进工作流。
- 参数：
  - type：object
  - properties：
    - result：
      - 类型：string
      - 描述：告诉用户你已经完成了什么工作。使用Markdown格式。

**change_pin_data_function**：（未提供详细信息）

**注意**：在使用代码时需要注意的事项

**输出示例**：模拟代码返回值的可能外观。
***
