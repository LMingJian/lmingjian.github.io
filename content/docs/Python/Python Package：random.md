---
title: Python Package：random
date: 2024-12-25T16:12:44+08:00
author: LiangMingJian
---

# 概述

random 是一个提供了多种随机数生成功能的模块。

# random.randrange(a, b, c)

返回给定范围列表 \[a, b, c) 内的随机整数，不包括b，c 为步长。

```
>>> random.randrange(1,100)
68
>>> random.randrange(1,100,3)  # 列表 [1,4,7,…,97]
16
```

# random.randint(a, b)

返回 \[1,100] 范围内的随机数整数，包括 100，第三个参数同 `random.randrange()` 用法一样。

```
>>> random.randint(1,100)
17
```

# random.random()

返回 \[0, 1) 范围内随机浮点数，不包括 1。

```
>>> random.random()
0.41385723239524297
```

# random.choice()

在给定容器中随机选择一个元素。

```
rand = ['0', '1', '2', '3', '4']
temp = choice(rand)
```

# random.sample(a)

在给定容器中随记选择特定数量元素。

```
>>> random.sample("abcde",2)
['e', 'b']  
```

# random.shuffle()

随机打乱传入的容器(容器必须是可变对象)。

```
>>> l = [1,2,3,4]
>>> random.shuffle(l)
>>> l
[1, 3, 2, 4]
```
