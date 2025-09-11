---
title: Python 的模块
date: 2024-12-25T16:10:46+08:00
author: LiangMingJian
---

# 概述

Module，模块，本质上就是一个编写好的 Python 文件。用户可以在这些文件内部提前编写好一些功能，然后在其他项目中，通过导入来复用这些功能。

在模块内部，通过全局变量 `__name__` 可以获取模块名，在其他项目中，通过模块名来导入模块。

# \_\_init\_\_.py

一个 Python 文件就是一个模块，如果将多个文件合并在一个文件夹，然后在文件夹内创建 `__init__.py` 文件，那么这个文件夹就会被识别为一个模块。

这一个文件夹模块，可以通过文件夹名来进行导入，然后通过“点式”来使用文件夹内的其他模块（Python 文件）。

这样的文件夹被称为模块包 module package，简称为包 package。

需要注意，`__init__.py` 往往只是一个空文件，但实际上它也可以执行一些包的初始化代码或设置一个 `__all__` 变量（`__all__` 是一个列表变量，里面包含了模块包内应该导入的模块名，在大多数情况下，该变量会自动生成）。

比如在 `__init__.py` 里面添加一个 `print("You have imported mypackage")`，那么当用户导入该文件所在包时，终端便会输出 `You have imported mypackage` 这串提示语句。

# 使用 import ... 来导入模块

```python
import module_name [as alias]
```

导入一个模块或模块包，然后通过"点式"来使用模块内的函数或类。

# 使用 from ... import ... 来导入模块内的函数或类

```python
from module_name import function_name [as alias]
```

从一个模块或模块包中导入一个函数或类，使得程序能直接使用该导入的函数或类，而不需要使用点式。

```python
from module_name import *
```

从一个模块或模块包中导入所有函数或类。

这里隐式使用了 `__all__` 变量，`__all__` 变量默认关联一个模块列表，在使用上述语句导入时，相当于导入列表中所有模块。

但需要注意的是，如果 `*` 中的模块还包含模块，那么该模块中的子模块将不会被导入。

# 模块的查找顺序

模块导入时会从 sys.path 返回的路径列表中按顺序寻找，sys.path 遵循顺序：

- 当前脚本所在目录
- 环境变量 PYTHONPATH 中的路径
- Python 标准库路径
- 第三方库安装路径 site-packages
