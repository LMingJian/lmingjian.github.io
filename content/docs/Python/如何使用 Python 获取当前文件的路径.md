---
title: 如何使用 Python 获取当前文件的路径
date: 2024-12-25T16:09:07+08:00
author: LiangMingJian
---

# 需求

在脚本中获取当前文件的路径。

# 通过`sys.path[0]`获取当前工作目录路径

该方法返回的是绝对路径。

```python
import sys
print(sys.path[0])
# >>> F:\Project
```

# 通过`__file__`获取文件路径

该方法的返回由系统决定是否是文件全名。

```python
print(__file__)
# >>> F:\Project\example.py
```

# 通过`os.path.abspath(__file__)`获取文件路径

该方法返回的是绝对路径。

```python
import os
print(os.path.abspath(__file__))
# >>> F:\Project\example.py
```

# 通过`os.path.split(os.path.realpath(__file__))`获取文件路径

该方法会将文件路径分成头和尾一对（文件目录，文件名），并生成二元元组返回。

```python
import os
print(os.path.split(os.path.realpath(__file__)))
# >>> ('F:\\Project', 'example.py')
```
