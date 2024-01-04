# FunctionDef import_all_modules_in_folder
**import_all_modules_in_folder函数**: 此函数的功能是导入指定文件夹中的所有Python模块。

该函数`import_all_modules_in_folder`接受两个参数：`file`和`name`。`file`参数是当前文件的路径，通常是`__file__`。`name`参数是当前模块的名称，通常是`__name__`。

函数首先使用`os.path.dirname`获取`file`所在的目录路径，然后创建一个空列表`all_modules`用于存储导入的模块。

接下来，函数遍历该目录下的所有文件和文件夹。对于每个条目，函数检查它是否是一个Python文件（以`.py`结尾且不是`__init__.py`）或者是一个包含`__init__.py`的目录（且不是`__pycache__`目录）。如果是，函数将从文件名或目录名构造模块的全名，格式为`name.module_name`。

使用`importlib.import_module`函数导入构造出的全模块路径，将导入的模块存储在`all_modules`列表中，并且将模块添加到全局命名空间中，这样就可以直接通过模块名访问模块。

最后，函数返回`all_modules`列表，其中包含了所有导入的模块对象。

**注意**:
- 使用此函数时，需要确保传入的`file`和`name`参数正确，否则可能导入错误的模块或者导入失败。
- 此函数会改变全局命名空间，导入的模块将直接通过模块名访问，需要注意命名冲突的问题。
- 如果目录结构发生变化，或者有新的模块被添加到目录中，需要重新运行此函数以导入新的模块。

**输出示例**:
假设当前目录下有两个Python文件`module1.py`和`module2.py`，调用`import_all_modules_in_folder(__file__, __name__)`后，函数可能返回如下列表：
```python
[<module 'your_package_name.module1' from '.../your_package_name/module1.py'>,
 <module 'your_package_name.module2' from '.../your_package_name/module2.py'>]
```
在这个例子中，`your_package_name`是当前模块的名称，`module1`和`module2`是导入的模块名。
***
