---
title: Python 的头信息
date: 2024-12-25T16:11:01+08:00
author: LiangMingJian
---

# 前言

在 Python 的代码文件中，支持设置一定的文档头信息，用来标注代码的编码环境、执行环境或作者等基础信息。

根据 Python PEP（Python 增强提案）所介绍，支持的文档头信息包含文件编码声明、解释器路径、元数据、模块说明符等几方面内容。

# PEP 394：解释器路径说明

[ PEP 394 – The “python” Command on Unix-Like Systems ](https://peps.python.org/pep-0394/)

```python
#!/usr/bin/env python3
```

在 Linux 系统中，操作系统会根据文件头（首行）的标记来判断文件类型，然后再去寻找文件所指定的程序来运行脚本。

因此 `#!/usr/bin/env python3` 就是告诉系统需要先到 env 中查找 Python 3 的安装路径，然后再去调用对应路径下的解释器程序完成操作。

通过写入程序解释器的执行路径，可以在预装 Python 2 的 Linux 系统中有效执行 Python 3 的文件，而不需要卸载原本的 Python 2，或将 Python 3 链接到 Python 上。

此外，在文件头中，还可以直接写入 `#!/usr/bin/python`，强制指定 Python 执行路径。

特别的，在 Windows 系统中，由于操作系统是根据文件名的后缀（扩展名）来判断文件类型的。因此，该文件头在 Windows 系统下相当于普通的注释，没有任何意义。

# PEP 263：文件编码说明

[ PEP 263 – Defining Python Source Code Encodings ](https://peps.python.org/pep-0263/)

```python
# -*- coding: utf-8 -*-
```

在 Python 2 中，文件一般默认的是 ASCII 码，如果文件里有中文时，运行会出现乱码，即使这个中文是注释。

因此，在文件头信息中写入 `# -*- coding:utf-8 -*-` 或 `coding=utf-8`，把文件编码指定为 UFT-8 就很有必要了。

注意，在 Python 3 中，文件的默认编码已经被改为 Unicode，也就是说，该文件头对于 Python 3 文件是无效的。

# PEP 257：文档字符串

[ PEP 257 – Docstring Conventions](https://peps.python.org/pep-0257/)

```python
"""  
我是一个模块  
这里是模块的提示信息  
"""
```

在 Python 文件的开头填入上述文档字符串，可用于标注该文件（模块）的名称和功能。

此外，在模块导入后，其能作为 `__doc__` 属性被识别。

```python
import example  
  
print(example.__doc__)
```

# PEP 8：元数据

[ PEP8 - Module Level Dunder Names ](https://peps.python.org/pep-0008/#module-level-dunder-names)

```python
__all__ = ['a', 'b', 'c']  
__version__ = '0.1'  
__author__ = 'Liang'  
  
def a():  
    pass  
  
def b():  
    pass  
  
def c():  
    pass
```

在 Python 文件的开头支持填入以上三种元数据，用以标注该文件（模块）所支持的所有方法，模块版本，模块作者信息。

此外，在模块导入后，其能作为对应属性被识别。

```python
import example  
  
print(example.__all__)  
print(example.__author__)  
print(example.__version__)
```

# Pycharm 特有

```python
# @Time : ${DATE} ${TIME} 自动填充创建时间 
# @Author: ${USER} 当前系统用户名 
# @File : ${NAME}.py 当前文件名 
# @Project: ${PROJECT_NAME} 当前项目名 
# @License: MIT
```

上述内容是 Pycharm 中支持设置的文件头信息，可以用来标注文件的创建时间，作者，文件名，项目名等内容。

**设置途径在：菜单 -> 文件（File）-> 设置（Setting）-> 编辑器（Editor）-> 文件和代码模板（File and Code Templates）-> Python Script。**

设置成功后，每当新建文件，Pycharm 会自动填入以上信息。
