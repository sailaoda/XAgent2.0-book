# ClassDef ENVIRONMENT
**ENVIRONMENT类的功能**：该类用于定义记录缓存的访问形式，决定了记录缓存的访问策略。

该类是一个枚举类，包含以下三个枚举值：
- Development：开发环境，不访问缓存，从头开始执行。
- Refine：优化环境，访问缓存，但要求用户消息必须一致，如果用户消息不一致（例如节点返回值发生变化），则停止访问缓存。
- Production：生产环境，无条件访问缓存，将记录重播一遍。

在代码中，可以通过ENVIRONMENT.Production等方式来使用这些枚举值。

**注意**：在使用该类时，需要根据具体的环境需求来选择合适的枚举值。在开发环境下，不访问缓存可以保证每次执行都是从头开始的；在优化环境下，可以根据用户消息的一致性来决定是否访问缓存；在生产环境下，无条件访问缓存可以提高执行效率。

该类在以下文件中被调用：
- XAgent/engines/dsl/DSL_runner/ProAgent/config.py：在该文件中的get_default_config函数中使用了ENVIRONMENT.Production作为默认的环境设置。
- XAgent/engines/dsl/DSL_runner/ProAgent/n8n_parser/compiler.py：在该文件中的tool_call_handle函数中根据ENVIRONMENT.Production来判断是否需要更新运行时。
- XAgent/engines/dsl/DSL_runner/ProAgent/running_recorder.py：在该文件中的query_llm_inout函数中根据不同的ENVIRONMENT值来决定是否访问缓存。

请注意：
- 该类是一个枚举类，用于定义记录缓存的访问形式。
- 在使用该类时，需要根据具体的环境需求来选择合适的枚举值。
***
