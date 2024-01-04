# FunctionDef add_to_map
**add_to_map 函数**: 此函数的功能是将一个键值对添加到全局字典中。

此函数 `add_to_map` 接受两个参数：`key` 和 `value`。它的主要作用是在一个名为 `global_map` 的全局字典对象中添加或更新键值对。当此函数被调用时，它会将传入的 `key` 作为字典的键，将 `value` 作为字典的值，并将这对键值对存储在 `global_map` 中。

详细代码分析如下：
- `global_map[key] = value`：这行代码是函数的核心，它表示将参数 `value` 赋值给 `global_map` 字典中 `key` 对应的条目。如果 `key` 已经存在于字典中，其对应的值将被新的 `value` 覆盖；如果 `key` 不存在，将创建一个新的键值对。

**注意**：
- 在使用此函数之前，需要确保 `global_map` 已经被定义为一个字典类型的全局变量，否则在执行此函数时会引发错误。
- 由于使用了全局变量，需要注意线程安全和并发访问的问题，以避免潜在的数据竞争或不一致性。
- 函数没有返回值，它的作用仅限于修改全局字典。
- 此函数没有进行任何类型检查或错误处理，因此在调用时需要确保传入的 `key` 和 `value` 是合适的数据类型，且 `key` 不应该是不可哈希的类型，如列表或字典。
***
# FunctionDef lookup_in_map
**lookup_in_map函数**: 该函数的功能是从一个全局映射中获取与指定键关联的值。

lookup_in_map函数是一个简单的查询函数，它接受一个参数`key`，这个参数代表要在全局映射`global_map`中查找的键。函数的主要目的是从`global_map`中检索与`key`对应的值。

详细代码分析如下：
- 函数定义为`def lookup_in_map(key):`，表明这是一个名为`lookup_in_map`的函数，它接受一个参数`key`。
- 在函数体内，使用了`global_map.get(key)`表达式来尝试从`global_map`中获取与`key`对应的值。
- `.get()`方法是Python字典对象的一个内置方法，它接受一个键作为参数，并返回与该键关联的值。如果字典中不存在该键，则返回`None`。
- 因此，`lookup_in_map`函数的返回值将是与`key`关联的值，或者如果`key`不存在于`global_map`中，则为`None`。

**注意**：
- 在使用`lookup_in_map`函数之前，确保`global_map`已经被正确初始化并填充了数据。
- 如果`global_map`中没有`key`对应的条目，函数将返回`None`。调用方需要对此进行适当的处理，以避免潜在的错误或异常。
- 由于该函数依赖于全局变量`global_map`，因此在并发环境下使用时需要注意线程安全问题。

**输出示例**：
假设`global_map`是如下的字典：
```python
global_map = {
    'apple': 'fruit',
    'carrot': 'vegetable',
    'python': 'programming language'
}
```
调用`lookup_in_map('apple')`将返回`'fruit'`。
调用`lookup_in_map('python')`将返回`'programming language'`。
调用`lookup_in_map('rock')`将返回`None`，因为`'rock'`不在`global_map`的键中。
***
