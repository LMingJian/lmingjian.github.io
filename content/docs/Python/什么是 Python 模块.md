---
title: 什么是 Python 模块
date: 2024-12-25T16:10:46+08:00
author: LiangMingJian
---

# 概述

Module，模块，本质上就是一个编写好的 Python 文件。用户可以在文件内部提前编写好一些功能，然后在其他项目中，通过导入来复用这些功能。在模块内部，通过全局变量 `__name__` 可以获取模块名（即字符串），在其他项目中，用户通过模块名来导入模块。

# \__init__.py

我们知道一个 Python 文件就是一个模块，但能不能将多个文件合并在一个文件夹内作为模块呢？答案显然是可以的。在文件夹通过创建 `__init__.py` 文件来告知 Python 这个文件夹将作为一个模块存在，用户可以通过导入文件夹名，然后通过点式使用文件夹内的模块（Python 文件）。这样的文件夹被称为模块包 module package，简称为包 package

`__init__.py` 可以只是一个空文件，但它也可以执行包的初始化代码或设置 `__all__` 变量。比如在`__init__`.py 里面加一个 `print("You have imported mypackage")`，那么当用户导入该文件所在包时，终端便会输出`You have imported mypackage`这串内容。

# \__all__

这是一个列表变量，里面包含了模块包内应该导入的模块名，在大多数情况下，该变量会自动生成。

# 使用 import ... 来导入模块

`import module_name [as alias]`，直接导入一个模块或模块包，然后通过点式来使用模块内的函数或类。

# 使用 from ... import ... 来导入模块内的函数或类

`from module_name import function_name [as alias]`，从一个模块或模块包中导入一个函数或类，能直接使用该导入的函数或类，不需要使用点式。

`from module_name import *`，从一个模块或模块包中导入所有函数或类。这里隐式使用了`__all__`变量，`__all__`默认关联一个模块列表，当执行`from ... import *`时，程序会导入列表中所有模块。但需要注意的是，如果`*`中的包含模块，那么该模块中的子模块将不会被导入。


sys.path 遵循顺序：

- 当前脚本所在目录
    
- 环境变量 PYTHONPATH 中的路径
    
- Python 标准库路径
    
- 第三方库安装路径 site-packages