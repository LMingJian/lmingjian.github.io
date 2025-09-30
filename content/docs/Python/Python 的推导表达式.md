---
title: Python 的推导表达式
date: 2025-09-30T12:01:42+08:00
author: LiangMingJian
---

# 概述

推导表达式是 Python 中用以快速简单生成列表，字典，集合，生成器等一种工具。

# 列表推导式

`[表达式 for 变量 in 迭代对象]` 或 `[表达式 for 变量 in 迭代对象 if 条件]`

该推导式会在执行后生成一个按表达式要求的列表。

```python
# 返回 0 到 9 的数值，并将数值乘 2
list1 = [i * 2 for i in range(10)]  
print(list1)
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# 返回 0 到 9 的偶数，并将数值乘 2
list1 = [i * 2 for i in range(10) if i % 2 == 0]  
print(list1)
# [0, 4, 8, 12, 16]
```

# 字典推导式

`{变量: 表达式 for 变量 in 迭代对象}` 或 `{变量: 表达式 for 变量 in 迭代对象 if 条件}`

该推导式会在执行后生成一个键是迭代对象变量，值是表达式要求的字典。

```python
# 返回 0 到 9 的数值，并将数值乘 2
dict1 = {i: i * 2 for i in range(5)}  
print(dict1)
# {0: 0, 1: 2, 2: 4, 3: 6, 4: 8}

# 返回 0 到 9 的偶数，并将数值乘 2
dict1 = {i: i * 2 for i in range(5) if i % 2 == 0}  
print(dict1)
# {0: 0, 2: 4, 4: 8}
```

# 集合推导式

`{表达式 for 变量 in 迭代对象}` 或 `{表达式 for 变量 in 迭代对象 if 条件}`

该推导式会在执行后生成一个按表达式要求的集合。

```python
set1 = {i * 2 for i in range(5)}  
print(set1)
# {0, 2, 4, 6, 8}

set1 = {i * 2 for i in range(5) if i % 2 == 0}  
print(set1)
# {0, 8, 4}
```

# 生成器表达式

`(表达式 for 变量 in 迭代对象)` 或 `(表达式 for 变量 in 迭代对象 if 条件)`

该推导式会在执行后生成一个按表达式要求的生成器。

> 生成器的详情介绍可查看：[ Python 的生成器 ](https://zhuanlan.zhihu.com/p/1956366187660312673)

```python
gen1 = (i * 2 for i in range(5))  
print(gen1)  
print(next(gen1))
# <generator object <genexpr> at 0x000001DE2B915150>
# 0

gen1 = (i * 2 for i in range(5) if i % 2 == 0)
print(gen1)  
print(next(gen1))
# <generator object <genexpr> at 0x000001DE2B915150>
# 0
```

# 拓展阅读：IF 条件推导式

在上述推导表达式中，都支持通过 if 进行条件判断。特别的，这里的 if 表达式也可以单独作为一个推导式使用，能实现更简单的代码结构。

在单独使用时，该条件表达式需要写成 `值1 if 条件 else 值2` 的三元结构。在条件真时，使用 if 前的值；在条件假时使用，else 后的值。

```python
x = 10  
res = "大于5" if x > 5 else "小于等于5"  
print(res)
# 大于 5
```
