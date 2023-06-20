---
title: Python 实现当前文件路径的获取
date: 2021-11-21
author: LM
---

## 1.需求

在脚本中获取当前文件的路径。

## 2.实现

### a.通过 `sys.path[0]`来获取文件当前工作目录路径（绝对路径）

```python
import sys
print(sys.path[0])
# >>> F:\Project
```

### b.通过 `__file__` 来获取文件所在的路径（由系统决定是否是全名）

```python
print(__file__)
# >>> F:\Project\example.py
```

### c.通过  `os.path.abspath(__file__)` 来获得文件所在的路径（绝对路径）

```python
import os
print(os.path.abspath(__file__))
# >>> F:\Project\example.py
```

### d.通过 `os.path.split(os.path.realpath(__file__))` 将文件路径名称分成头和尾一对，生成二元元组（文件目录，文件名）

```python
import os
print(os.path.split(os.path.realpath(__file__)))
# >>> ('F:\\Project', 'example.py')
```

