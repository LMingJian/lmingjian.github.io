---
title: 修复 Python 字典在遍历时不能进行修改的问题
date: 2021-03-31
author: LMingJian
---

## BUG 描述

如下面的代码（遍历某个字典的键，删除某个键的值），在执行时会出现报错。

```python
a={'a':1, 'b':0, 'c':1, 'd':0}
for key in a.keys():
	del a[key]
```

## Resolution

字典在遍历时不能被修改删除，需要将字典转成列表或集合再进行遍历。

```python
a={'a':1, 'b':0, 'c':1, 'd':0}
for key in list(a.keys()):
	del a[key]
```

