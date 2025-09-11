---
title: Python 的函数定义方法
date: 2025-07-07T11:39:21+08:00
author: LiangMingJian
---

# 函数的定义

```python
def func(key1, key2, key3):
    pass
```

形如上述结构（使用关键字 def，后面有着函数名与括号内的形参列表，函数体从下一行开始，且缩进）的代码段，在 Python 中被称之为函数，也可以称为方法。

# 默认值参数

在 Python 的函数定义过程中，用户可以为形参设置默认值，使得函数在调用时，可以传递更少的实参（没传递的实参使用对应形参设置的默认值作为数据）。

特别的，我们可以称**没有默认值的参数为必选参数，具有默认值的参数为可选参数**。

比如下述代码：

```python
def func(key1, key2=2, key3=3):  
    pass

func(1)
# 1 个必选参数，2 个可选参数
```

需要注意，**在 Python 的函数定义时，可选参数（默认值参数）必须跟随在必选参数（非默认值参数）后面**。即不能出现 `func(key1=1, key2, key3)` 这一种函数定义方式。

# 通过位置顺序传递实参

在 Python 中，默认的实参传递方式是位置顺序，即按函数定义时设置的形参列表顺序来传递实参。

比如下述代码，实参 1，2，3 分别按顺序传递给 key1，key2，key3：

```python
def func(key1, key2, key3):  
    print(key1)
    print(key2)
    print(key3)

func(1, 2, 3)
# 结果：打印 1，2，3
```

# 通过关键字传递实参

在 Python 中，除了通过位置顺序传递实参，还可以通过 `kwarg=value` 关键字来灵活的传递实参。

比如下述代码，实参 2，3 通过关键字传递 key2，key3：

```python
def func(key1, key2, key3):  
    print(key1)
    print(key2)
    print(key3)

func(key1=1, key2=2, key3=3)
# 结果：打印 1，2，3
```

关键字参数在使用时具有以下要求：

1. **关键字参数必须在位置参数后面**，形如 `func(key1=1, 2, 3)` 是**错误**的。
2. **关键字参数传递的顺序可以打乱**，形如 `func(1, key3=3, key2=2)` 是**正确**的。
3. **不能对同一个参数多次赋值**，形如 `func(1, key1=1)` 是**错误**的。

# 通过 `/` 和 `*` 来限制实参的传递方式

默认情况下，参数可以按位置或关键字传递给 Python 函数。但是为了让代码易读、高效，最好限制参数的传递方式，这样，开发者只需查看函数定义，就可以确定参数是仅按位置，还是仅按关键字传递。

```python
def func(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
        -----------    ----------     ----------
        |             |               -- 仅限关键字
        |             -- 位置或关键字
        |                             
        -- 仅限位置
```

`/` 和 `*` 是可选的，通过在定义时设置 `/` 和 `*`，可以限定参数的传递方式。

**没有设置 `/` 和 `*` 或在 `/` 和 `*` 之间的参数**

参数可以使用位置，也可以使用关键字传递给函数，但需要注意，位置参数必须先于关键字参数使用。

**`/` 前仅限位置参数**

定义在 `/` 前的形参都只能使用位置参数传递，如果使用关键字则失败。

```python
def func(pos1, pos2, /):
    pass

func(1, 2)  # 正确的
func(pos1=1, pos2=2)  # 错误的
"""报错
Traceback (most recent call last):
  File "F:\MyPython\PythonProjectExample\example.py", line 4, in <module>
    func(pos1=1, pos2=2)
TypeError: func() got some positional-only arguments passed as keyword arguments: 'pos1, pos2'
"""
```

**`*` 后仅限关键字参数**

定义在 `*` 后的形参都只能使用关键字参数传递，如果使用位置参数则失败。


```python
def func(*, kwd1, kwd2):
    pass

func(kwd1=1, kwd2=2)  # 正确的
func(1, 2)  # 错误的
"""报错
Traceback (most recent call last):
  File "F:\MyPython\PythonProjectExample\example.py", line 4, in <module>
    func(1, 2)
TypeError: func() takes 0 positional arguments but 2 were given
"""
```

# 实参列表 `*args`

在函数定义时，如果使用 `*args` 的格式定义一个形参（这里的 args 可以是其他英文字符串，`*` 的意义是解包列表），那么这个形参则支持传递一个可变参数列表。

**通过 `*args` 的使用，可以让用户在调用函数时使用任意数量的位置实参，而这些实参最终会被包含在一个元组中。**

```python
def func(*args):  
    print(type(args))  
    for each in args:  
        print (each)  
  
func(1,2,3)
# 输出：<class 'tuple'> 和 1，2，3
```

需要注意，`*args` 形参后的所有参数只能是关键字参数，即下面这种用法是禁止的：

```python
def func(*args, key):  
    pass

func(1, 2, 3, key=1)  # 正确的
func(1, 2, 3, 4)  # 错误的
"""报错
Traceback (most recent call last):
  File "F:\MyPython\PythonProjectExample\example.py", line 4, in <module>
    func(1, 2, 3, 4) 
TypeError: func() missing 1 required keyword-only argument: 'key'
"""
```

# 实参字典 `**kwargs`

在函数定义时，如果使用 `**kwargs` 的格式定义一个形参（这里的 kwargs 可以是其他英文字符串，`**` 的意义是解包字典），那么这个形参则支持传递一个可变参数字典。

**通过 `**kwargs` 的使用，可以让用户在调用函数时使用任意数量的关键字实参，而这些实参最终会被包含在一个字典中。**

```python
def func(**kwargs):  
    print(type(kwargs))  
    for each in kwargs:  
        print(f'{each}: {kwargs[each]}')  
  
func(key1=1, key2=2, key3=3)
# 输出：<class 'dict'> 和 key1: 1，key2: 2，key3: 3
```

特别的，`*args` 和 `**kwargs` 往往一起使用，用以获取可能传递给函数的所有剩余参数。其格式为 `func(value, *args, **kwargs)`。

# 拓展阅读：`*` 和 `**` 操作符

`*` 在 Python 中是用于解包列表或元组的一个操作符，其作用是将列表和元组中的参数依次取出并传递。

比如在函数调用时，可以使用 `*` 将一个列表的元素作为函数实参传递。

```python
def func(key1, key2, key3):  
    pass  
  
data = [1, 2, 3]  
func(*data)
```

`**` 也是类似功能，只不过 `**` 针对的是字典，其作用是将字典的元素依次按关键字的形式取出并传递。 

```python
def func(key1, key2, key3):  
    pass  
  
data = {'key1': 1, 'key2': 2, 'key3': 3}  
func(**data)
```

# 拓展阅读：类型注解

对于整型，字符，浮点这类数据，在函数定义时可以像 C，C++ 一样进行类型注解，限定其使用的数据类型。

```python
def func(key1: int, key2: str, key3: float) -> None:  
    pass

def func(key1: int, key2: str, key3: float) -> int:  
    pass
```

对于列表，字典这类数据，除了外部的类型注解外，内部元素也支持进行注解。

```python
from typing import List, Dict  
  
def func(key1: List[int]) -> Dict[str, int]:  
    pass
```

========

> [ 官方文档 ](https://docs.python.org/zh-cn/3.13/tutorial/controlflow.html#defining-functions)