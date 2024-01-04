# ClassDef InterruptMessage
**InterruptMessage类的功能**: 这个类表示中断消息。

参数:
- message (str): 中断的消息。
- response (str): 对中断的响应。
- stop (bool): 是否停止当前任务。
- rectify_later_plan (bool): 是否需要修正后续计划。

这个类用于处理中断消息。它包含了中断的消息内容、对中断的响应、是否停止当前任务以及是否需要修正后续计划等属性。通过设置这些属性，可以控制中断的行为和后续计划的执行。

在XAgent/engines/plan.py文件中的listen_interruption方法中，通过创建InterruptMessage对象来处理中断消息。当检测到中断信号时，会创建一个InterruptMessage对象，并将其添加到interrupt_msg_buffers列表中。然后，会更新长期上下文和执行计划，以处理中断消息。

在XAgent/engines/react.py文件中的step方法中，当检测到中断信号时，会创建一个InterruptMessage对象，并将其添加到exec_node的interrupts列表中。然后，根据中断的属性，决定是否需要修正后续计划或停止当前任务。如果需要修正后续计划，会调用plan_rectify方法进行修正。如果需要停止当前任务，会发送一个系统消息，并将当前任务提交为失败状态。

在XAgent/models/graph.py文件中的ExecutionNode类中，interrupts属性是一个InterruptMessage对象的列表，用于存储中断消息。当执行节点遇到中断时，会创建一个InterruptMessage对象，并将其添加到interrupts列表中。

**注意**: 在使用InterruptMessage类时，需要注意设置message、response、stop和rectify_later_plan属性，以控制中断的行为和后续计划的执行。
***
