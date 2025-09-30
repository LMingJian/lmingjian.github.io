---
title: Python 的打印输出 Print
date: 2025-09-15T10:29:32+08:00
author: LiangMingJian
---

# 前言

在 Python 中，通过 print 方法可以将输入的数据打印到标准输出（一般是控制台）。

# print 的结构

`print(*objects, sep=' ', end='\n', file=None, flush=False)`

## \*objects

用户需要打印的数据。它可以是一个数据，也可以是多个数据。它可以是字符串，也可以是整型，字典，列表这些对象。这些对象在打印时，有的会直接转换为字符串，有的会调用对象内部的 `__str()__` 方法进行输出。

```python
>>> print(1)
>>> 1
>>> print('A', 2, [])
>>> A 2 []
```

## sep

分隔符。当在调用 print 时，使用关键字参数给出分隔符，那么传入的多个数据会被使用的分隔符进行分隔输出。默认的分隔符是空格。

```python
>>> print('A', 2, [])
>>> A 2 []
>>> print('A', 2, [], sep=',')
>>> A,2,[]
```

## end

输出结尾，所有的输出都会自动添加这个结尾。默认是换行符 `\n`，即每次输出都自动换行。如果不想换行则，使用空输入 `''`。

```python
print(1)
print(2)
"""
1
2
"""

print(1, end='')
print(2, end='')
"""
12
"""
```

## file

输出文件，所有的输出都会指向到这个地方。默认为 None 时，输出会被指向到标准输出 `sys.stdout`。file 可以被指向到一个可写的文件对象，这个文件对象要求可以正常执行 `write(string)` 方法。

需要注意，因为 print 方法会将所有输入转换成字符串输出，因此 file 指向的文件对象不能是二进制对象（即文件不能是 wb 模式读取的）。

```python
import sys

# 以下两个代码是等价的
print(1)
print(1, file=sys.stdout)

# 输出到一个文件对象，模式不能是二进制 b
with open("example.txt", "w") as f:  
    print(1, file=f)
```

## flush

缓冲刷新。默认为 Flase 时，输出的刷新会有系统自动管理，Python 的标准输出使用行缓冲（遇到换行符 `/n` 输出一次）和全缓冲（缓冲区满了输出）。通过设置为 True，可以让 Python 无视上面两个机制，实现实时输出，可以用于进度条的实现。

> 注意，在 Python 3.13 版本，flush 不管是不是 True，每次 print 都会立马执行一次输出，不会出现上述问题。实际支持这个的版本暂时无法确定。

> 此外，下述实验代码在 Python 3.6 版本是有效的。

```python
import time

# 不使用 flush=True
for i in range(5):  
    print(i, end='---')  
    time.sleep(1)
# 上面执行内容会在 5s 后一起输出
# 0---1---2---3---4---

# 使用 flush=True
for i in range(5):  
    print(i, end='---', flush=True)  
    time.sleep(1)
# 上面执行内容会实时输出
# 0---1---2---3---4---
```

# print 的技巧

## 不换行打印

```python
# 使用 end 指定空结尾
print(1, end='')  
print(2, end='')  
print(3, end='')
# 123
```

## 多个数据输出时使用分隔

```python
print(1, 2, 3, sep=' | ')
# 1 | 2 | 3
```

## 长行输出

```python
# 使用反斜杠分割长行字符串，每行字符必须顶格
# 分割后禁止将代码对齐，对齐后会导致对齐填入的空格也被输出。
print("Python 是一门易于学习、功能强大的编程语言。\  
它提供了高效的高级数据结构，还能简单有效地面向对象编程。\  
Python 优雅的语法和动态类型以及解释型语言的本质。\  
使它成为多数平台上写脚本和快速开发应用的理想语言。")

# 通过字符串拼接输出
print("Python 是一门易于学习、功能强大的编程语言。",  
      "它提供了高效的高级数据结构，还能简单有效地面向对象编程。",  
      "Python 优雅的语法和动态类型以及解释型语言的本质。",  
      "使它成为多数平台上写脚本和快速开发应用的理想语言。")
```

## 单双引号打印

```python
# 在要输出单引号时，使用双引号包裹输出内容
print("输出一个 ' 号")

# 在要输出双引号时，使用单引号包裹输出内容
print('输出一个 " 号')

# 直接使用转义字符
print('输出一个 \" 和 \' 号')
```

> 支持的转义字符可查阅：[ 转义字符 ](https://docs.python.org/zh-cn/3.13/reference/lexical_analysis.html#escape-sequences)

## 输出到文件

```python
# 输出到一个非二进制打开的文本对象
with open("example.txt", "w") as f:  
    print('Hello Python', file=f)
```

## 格式化输出

```python
data = 1
print(f'Number is {data}')
# Number is 1
```

> 更多可查看：[ 如何使用 Python f-string 格式化字符串 ](https://zhuanlan.zhihu.com/p/1944049461090289386)
