---
title: Python 的注释标记
date: 2024-12-25T16:10:40+08:00
author: LiangMingJian
---

# 前言

在 Python 中，支持使用一些如 `#noqa`，`#todo` 这样的注释标记。这些注释标记能更好的提高代码的可读性，同时在 IDE，如 PyCharm 中，这些标记还会有特殊的功能。

# 代码检查类

## `# noqa`

让静态代码检查工具忽略这行的所有警告。

比如下面的 `except` 存在警告，此时可以在后面添加注释 `# noqa` 让编译器忽略这个警告。

![](/_images/drawingbed/img/202508281023141.png)

![](/_images/drawingbed/img/202508281023642.png)

在 PyCharm 中，`# noqa` 能忽略**错误**级下的所有警告。

![](/_images/drawingbed/img/202508281024682.png)

特别的，可以使用 `# noqa: 错误码` 来忽略特定类型的警报，比如下面出现未使用的 import 语句，那么我们可以使用 `# noqa: F401` 单独忽略这一警报。

![](/_images/drawingbed/img/202508281025577.png)

![](/_images/drawingbed/img/202508281025716.png)

对应错误的错误码可以在静态代码检查工具 Flake8，Pylint 的文档中查找。

> [Pylint](https://pylint.pycqa.org/en/latest/user_guide/checkers/features.html#pylint-features)  
> [Flake8](https://flake8.pycqa.org/en/latest/user/error-codes.html)

## `# noinspection` 检查名称

忽略某些代码检查警告。

和 `# noqa` 不同，`# noinspection` 需要指定检查名称，它能对特定检查规则进行忽略，不局限于一种错误类型，同时它不使用错误码，而是检查名称，使得代码更易读，比如 `# noinspection PyBroadException` 这个可以忽略“过于宽泛的异常捕获”这一类型的错误。

![](/_images/drawingbed/img/202508281027884.png)

![](/_images/drawingbed/img/202508281027393.png)

PyCharm 支持的检查类型请查看：

> [Disabling and enabling inspections | PyCharm](https://www.jetbrains.com/help/pycharm/disabling-and-enabling-inspections.html#comments-ref)

# 任务标记类

## `# TODO`

标注为需要完成的功能。

![](/_images/drawingbed/img/202508281027964.png)

在 PyCharm 中，可以在侧边栏的 TODO 工具中，可以快速定位所有具有这个标识的代码。

![](/_images/drawingbed/img/202508281028455.png)

## `# FIXME`

标注为需要修正的功能。

![](/_images/drawingbed/img/202508281028239.png)

同样的，在 PyCharm 侧边栏的 TODO 工具中，可以快速定位所有具有这个标识的代码。

![](/_images/drawingbed/img/202508281029832.png)

# 文档生成类

## 文件级/模块级文档

在 Python 文件的开头标注模块信息，在其他文件导入时进行提示。

```python
"""模块的简要描述

这里是模块的详细说明
"""
import sys

def func():
    pass
```

![](/_images/drawingbed/img/202508281030094.png)

## 函数级/方法级文档

在函数的定义后面标注函数信息，在其他地方调用时即可查看提示。

**Google 风格**

```python
def func(x, y, c):
    """功能说明
    
    Args:
        x (int): 参数说明
        y (int): 参数说明
        c (str): 参数说明
    Returns:
        str: 返回值说明
    Raises:
        ValueError: 参数无效时触发
    """
    return f'{c}: {x + y}'
```

![](/_images/drawingbed/img/202508281030990.png)

**Sphinx 风格**

```python
def func(x, y, c):
    """功能说明

    :param x: 参数说明
    :type x: int
    :param y: 参数说明
    :type y: int
    :param c: 参数说明
    :returns: 返回值说明
    :rtype: str
    :raises ValueError: 异常说明
    """
    return f'{c}: {x + y}'
```

![](/_images/drawingbed/img/202508281030354.png)

**Numpy 风格**

```python
def func(x, y, c):
    """功能说明

    Parameters
    ----------
    x : int
        参数详细说明
    y : int
        参数详细说明
    c
        参数详细说明
    Returns
    -------
    str
        返回值详细说明
    Raises
    -------
    ValueError
        异常说明
    """
    return f'{c}: {x + y}'
```

![](/_images/drawingbed/img/202508281031926.png)

## 类级文档

在类定义后面标注类基础信息，在实例化类时可以查看提示。

```python
class MyClass:
    """功能描述

    类的详细说明

    Attributes:
        x (int): 属性说明
        y (int): 属性说明
        z: 属性说明
    """

    def __init__(self, x, y, z):
        self.x = x
        self.y = y
        self.z = z
        pass

    def func(self):
        pass
```

![](/_images/drawingbed/img/202508281031743.png)

# 强制类型注解

## 变量类型提示

强制指定变量的类型，当错误类型被赋值时，IDE 会提示错误。

```python
a = []     # type: list[str]
b = 1      # type: int
c = 'str'  # type: str
```

![](/_images/drawingbed/img/202508281032339.png)

![](/_images/drawingbed/img/202508281032075.png)

## 参数类型提示

```python
def func(x: int, y: str, z: list, c: dict) -> str:
    pass
```

![](/_images/drawingbed/img/202508281033973.png)

![](/_images/drawingbed/img/202508281033682.png)
