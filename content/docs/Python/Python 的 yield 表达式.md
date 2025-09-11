---
title: Python 的 yield 表达式
date: 2024-12-25T16:11:20+08:00
author: LiangMingJian
---

# yield 表达式

当用户在一个函数体内使用 yield 表达式，那么这个函数会变成一个生成器函数。

```python
def gen():  # 定义一个生成器函数
    yield 123
```

生成器（generator）函数是用来返回一个生成器迭代器（generator iterator）对象的函数。

生成器迭代器要求程序在执行到每个 yield 时，临时暂停处理过程，并记住执行状态（包括局部变量和挂起的 try 语句），同时将一个值通过 yield 返回，最后等待用户唤醒程序，继续执行。当程序继续执行时，如果再次遇到 yield，则重复暂停操作，重新等待运行。

用户可以通过 `next()` 函数来恢复生成器的运行，重新运行后，与普通函数不同，生成器函数会从暂停位置继续执行，而不是从新开始。

**特别注意，生成器函数的启动也需要使用 `next()` 函数。 **

比如下述代码，生成器函数通过 `next()` 启动，当执行到第一个 yield 时，暂停程序，然后返回一个值。当再次通过 `next()` 启动时，程序不是从头开始的，而是从上次暂停的 yield 后继续执行，以此往复。

```python
def func():  
    print("程序开始...")  
    yield 1  
    print("程序继续...")  
    yield 2  
    print("程序结束...")  
    yield 3  
  
foo = func()  
print(next(foo))  # 输出：程序开始... 和 1
print(next(foo))  # 输出：程序继续... 和 2
print(next(foo))  # 输出：程序结束... 和 3
```

# 拓展阅读：为什么需要 yield？

yield 能通过状态保持和暂停恢复的特性，在大型文件处理，流式数据处理，无限序列循环的过程中分段分批次读取或处理数据，显著的优化内存，提高程序效率。

比如在循环 `for i in range(1000)` 中，由于 range 会根据实参生成一个给定长度列表，当 range 的长度很大时，程序的内存占用也就很高，极其容易造成资源的浪费。

如果使用 yield 生成 generator iterator 迭代对象，则不会出现这个问题。

比如下述代码，程序通过 yield 保持每次循环状态，并返回数据，不需要在一开始生成一个大内存的列表，只占用 temp 这一个变量的内存，内存占用极小：

```python
def func():  
    temp = -1  
    while temp < 1000:  
        temp += 1  
        yield temp  
  
foo = func()  
print(next(foo))  # 返回 0
print(next(foo))  # 返回 1  
print(next(foo))  # 返回 2
```

————————————

> [ yield 语句-官方文档 ](https://docs.python.org/zh-cn/3.13/reference/simple_stmts.html#the-yield-statement)
> 
> [ yield 表达式-官方文档 ](https://docs.python.org/zh-cn/3.13/reference/expressions.html#yield-expressions)
