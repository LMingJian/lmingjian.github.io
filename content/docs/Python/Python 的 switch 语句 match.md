---
title: Python 的 switch 语句 match
date: 2025-09-01T09:06:13+08:00
author: LiangMingJian
---

# 前言

在 C，C++，Java 这些程序设计语言中，支持使用 `switch ... case ...` 语句来编写多分支选择结构，根据某一个值的内容，分别执行不同的代码。

```C
switch(value) { 
    case 1: 
        value = 1;
        break;
    case 2:
        value = 2;
        break;
    default: 
        value = 3;
}
```

在 Python 以前的版本中，`switch ... case ...` 语句是不被支持的，**直到 3.10 版本，[PEP 636](https://peps.python.org/pep-0636/)**，与 `switch ... case ...` 语句类似的 `match ... case ...` 被成功引入，使得 Python 可以使用多分支选择结构。

# match 语句

**再次提醒：match 语句在 Python 3.10 后才能使用，较低版本的 Python 无法使用！！！**

## 基本使用

```python
match value:  
    case 1:  
        print(value)  
    case 2:  
        print(value)  
    case _:  
        print(value)
```

如上述代码，match 语句支持接受一个表达式，然后把它的值**按顺序**与一个或多个 case 块进行比较。根据比较结果，程序执行相应的代码。

在最后一个 case 中，**变量 `_` 表示通配符，该通配符必定被匹配成功。**在 Python 中，这个变量 `_` 类似 `switch ... case ...` 中的 `default`，用以在没有匹配值时执行。

当 `case` 没有匹配到任何值，且不存在变量 `_`  时，程序不会执行任何分支，该 match 不会输出任何内容。

## 支持“或”运算

在 match 语句中，**支持使用“或”运算 `|` 来将多个值运用到 case 块中**，其代码如下：

```python
match value:  
    case 1 | 2:  
        print(value)  
    case 3:  
        print(value)  
    case _:  
        print(value)
```

## 多维数据的解包匹配

在 match 语句中，匹配的对象不仅仅只有数值与字符，对于多维数据，match 提供高级的解包模式来匹配对应数据。

**形如 `(x, y), (x, y, z), [x, y], [x, y, z]` 的数据我们可以称之为多维数据（列表，元组），match 支持匹配其中的一个值，也可以匹配其中的所有值。**

比如下述代码：

```python
match value:
    case [0, 0]:
        print("Origin")
    case [0, y]:
        print(f"Y={y}")
    case [x, 0]:
        print(f"X={x}")
    case [x, y]:
        print(f"X={x}, Y={y}")
    case _:
        raise ValueError("Not a point")
```

- `[0, 0]`：当 X，Y 都等于 0 时，即`[0, 0]`，输出 Origin；
- `[0, y]`：当只有 X 等于 0 时，**Y 通配**，如 `[0, 1]`，输出 Y 对应的值；
- `[x, 0]`：当只有 Y 等于 0 时，**X 通配**，如 `[1, 0]`，输出 X 对应的值；
- `[x, y]`：**当 X，Y 都通配时**，比如 `[1, 1]`，输出 X 和 Y 的值；
- `_`：当 value 不是一个二维数据时，比如 `[1], [1, 2, 3]`，抛出错误。

**特别的，对于更多维的数据，我们可以使用 `*_` 或 `*rest` 来忽略不需要的多维数据，使得代码更为简洁。**

比如 `[x, *_]` 即可以匹配 `[x, y]` 的数据，也可以匹配 `[x, y, z]` 的数据。

## 字典数据的匹配

**字典（映射）的匹配逻辑与多维数据的匹配逻辑类似，但字典的匹配必须匹配到“键”。**

比如下述代码：

```python
data = {"type": "user", "name": "Alice", "age": 25}
match data:
    case {"type": "user", "name": name, "age": age}:
        print(f"用户: {name}, 年龄: {age}")  
        # 输出: 用户: Alice, 年龄: 25
    case {"type": "product", "name": name}:
        print(f"产品: {name}")
    case _:
        print("未知类型")
```

在匹配时，键是必须的，值可以像多维数据匹配时，使用变量进行通配。

**同样的，对于更多的键值对，我们可以使用** `**rest` **忽略不需要的键值对**（注意 `**_` 在这里是禁止使用的）。**

比如 `{"type": "user", "name": name, **rest}`。

## 守护模式

在 case 捕获数据后，我们可以使用 if 语句来进行守护，只有**当 if 判断成功后才执行后续相应代码。**

比如下述代码：

```python
http = {'status': 500}
match http:
    case {"status": status} if status >= 400:
        print(f"错误状态码: {status}")
    case {"status": status}:
        print(f"正常状态码: {status}")
```

当 status 大于等于 400 时，输出错误状态码，当小于 400 时，输出正常状态码。

——————

> 详细文档请查看：[ match 语句-官方文档 ](https://docs.python.org/zh-cn/3.13/tutorial/controlflow.html#match-statements) 或 [ PEP-636 ](https://peps.python.org/pep-0636/)
