---
title: Python 模块：random
date: 2021-03-29
author: LM
---

## 1.简介

Python 中一个提供生成随机数函数的模块。

## 2.使用：random.randrange(a, b, c)

返回给定范围列表 [a, b, c) 内的随机整数，不包括b，c 为步长

```
>>> random.randrange(1,100)
68
>>> random.randrange(1,100,3)  # 列表 [1,4,7,…,97]
16
```

## 3.使用：random.randint(a, b)

返回 [1,100] 范围内的随机数整数，包括100，第三个参数同random.randrange()用法一样

```
>>> random.randint(1,100)
17
```

## 4.使用：random.random()

返回 [0, 1) 范围内随机浮点数，不包括1

```
>>> random.random()
0.41385723239524297
```

## 5.使用：random.choice()

在给定容器中随机选择一个元素

```
rand = ['0', '1', '2', '3', '4']
temp = choice(rand)
```

## 6.使用：random.sample(a)

在给定容器中随记选择特定数量元素

```
>>> random.sample("abcde",2)
['e', 'b']  
```

## 7.使用：random.shuffle()

随机打乱传入的容器(容器必须是可变对象)

```
>>> l = [1,2,3,4]
>>> random.shuffle(l)
>>> l
[1, 3, 2, 4]
```

