---
title: Python 的枚举类 Enum
date: 2025-09-01T10:39:32+08:00
author: LiangMingJian
---

# 前言

在 C 语言中，对于一组存在关联关系的常量数据，如红绿蓝三色，我们往往可以使用枚举类型 enum 全部命名并设置数值。在使用时，可以直接通过枚举名调用枚举值使用，提高了程序的可读性和维护性。

比如下面代码所示：

```C
#include <stdio.h>

enum Color { RED = 1, GREEN = 2, BLUE = 3 };

int main() { 
    enum Color c = BLUE; 
    printf("Color code: %d\n", c); // 输出 2 
    return 0; 
}
```

在 Python 中，虽然不具备像 C 一样的枚举类型，但也提供了类似的枚举类 Enum 供开发者使用。通过继承或实例化枚举类可以实现与 C 枚举类型一致的功能。

# Enum 标准枚举类

Enum 是标准类，允许传入各种类型的数据。

在使用过程中，我们一般推荐使用类继承的方式来建立一个枚举，当然使用实例化的方式也支持，但没有类继承方式那般可读性良好。

比如下面代码：

```python
from enum import Enum

# 通过类继承使用
class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

# 调用
print(ColorA.RED.name)   # RED
print(ColorA.RED.value)  # 1

# 通过实例化使用
Color = Enum('Color', [('RED', 1), ('GREEN', 2), ('BLUE', 3)])

# 调用
print(ColorB.GREEN.name)   # GREEN
print(ColorB.GREEN.value)  # 2
```

# IntEnum 整数枚举类

特别的，如果需要指定数据类型，可以使用相似的其他类，比如 IntEnum 整数枚举类。

IntEnum 的特点是可以直接进行**计算**，但结果会丢失枚举类型，变为整型。

```python
from enum import IntEnum  
  
class Number(IntEnum):  
    ONE = 1  
    TWO = 2  
    THREE = 3  
  
res = Number.ONE + Number.TWO  
print(res)        # 3
print(type(res))  # <class 'int'>
```

# StrEnum 字符枚举类

StrEnum 限定数据是字符的枚举类，其同样也支持相加，但结果会是字符的拼接。

```python
from enum import StrEnum  
  
class Color(StrEnum):  
    RED = 'r'  
    GREEN = 'g'  
    BLUE = 'b'  
  
res = Color.GREEN + Color.BLUE  
print(res)        # gb
print(type(res))  # <class 'str'>
```

# Flag 旗帜枚举类

Flag 是支持使用位运算 `&` (AND)，`|` (OR)，`^` (XOR) 和 `~` (INVERT) 的一种枚举类。其数值要求是整型，一般设置为 2 的幂次方 1，2，4，8，方便进行位运算。大部分情况下，可以使用 `auto()` 进行自动生成。

Flag 旗帜枚举类往往用于组合，判断某个枚举类型是否在某个组合之中。

```python
from enum import Flag, auto  
  
class Color(Flag):  
    RED = auto()  
    GREEN = auto()  
    BLUE = auto()  
  
purple = Color.RED | Color.BLUE  # noqa
print(Color.GREEN in purple)
```

# IntFlag 整型旗帜枚举类

与 Flag 枚举类类似，同样支持位运算，但额外支持整数运算，但在整数运算后会变为整型。

```python
from enum import IntFlag, auto  
  
class Color(IntFlag):  
    RED = auto()  
    GREEN = auto()  
    BLUE = auto()  

# 位运算用法
purple = Color.RED | Color.BLUE  
print(Color.GREEN in purple)  

# 整数运算用法
res = Color.RED + 1  
print(res)  
print(type(res))
```

# verify 装饰器

Enum 枚举类支持使用 verify 装饰器来对数据进行约束，支持的约束类型包括：UNIQUE（每个值只有一个名称），CONTINUOUS（最低和最高值间必须连续，不能缺失），NAMED_FLAGS（确保旗帜符合生成规范是 2 的幂次方）。

```python
from enum import Enum, verify, UNIQUE

@verify(UNIQUE)
class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3
    CRIMSON = 1
# 重复值，所以失败
# ValueError: aliases found in <enum 'Color'>: CRIMSON -> RED

@verify(CONTINUOUS)
class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 5
# 值不连续，所以失败
# ValueError: invalid enum 'Color': missing values 3, 4

@verify(NAMED_FLAGS)
class Color(Flag):
    RED = 1
    GREEN = 2
    BLUE = 4
    WHITE = 15
# 值不规范，所以失败
# ValueError: invalid Flag 'Color': aliases WHITE is missing combined values of 0x18
```

# auto 自动填充方法

`auto()` 方法可自动填充数据。对于 IntEnum 它会以初始值自动加一。对于 Flag 和 IntFlag 它会是 2 的幂次方。对于 StrEnum 它会是成员名称的小写。

```python
from enum import IntFlag, auto  
  
class Color(IntFlag):  
    RED = auto()  
    GREEN = auto()  
    BLUE = auto()  
```

————————————

> [ enum --- 对枚举的支持 ](https://docs.python.org/zh-cn/3.13/library/enum.html)
