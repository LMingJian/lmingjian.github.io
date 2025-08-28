---
title: 如何使用 Python 获取当前文件的路径
date: 2024-12-25T16:09:07+08:00
author: LiangMingJian
---

# 需求

在脚本中获取当前文件的路径。

# 通过`__file__`获取文件路径

`__file__` 根据文件的加载方式，有可能输出的是相对路径，也有可能输出绝对路径。因此建议在使用时统一利用 `os.path` 进行格式化。（这在 Linux 系统上应当注意）

```python
import os 

current_file_path = os.path.abspath(__file__) 
current_dir_path = os.path.dirname(__file__) 

print(current_file_path) 
print(current_dir_path)

"""
>>> F:\Project\example.py
>>> F:\Project
"""
```

# 使用 `sys.argv[0]` 获取

`sys.argv` 是 Python 中一个特别的内置列表，它用来获取命令行参数，即通过命令行调用脚本时传入的参数列表，比如：`-h`，`-v`，`-a`。

`sys.argv` 遵循顺序：

- Python 脚本的全称
- 用户输入的参数

显然，通过读取列表第一个值，我们可以间接获取到当前文件的路径。

```python
import os
import sys

current_file_path = os.path.abspath(sys.argv[0])
current_dir_path = os.path.dirname(sys.argv[0])

print(current_file_path)
print(current_dir_path)

"""
>>> F:\Project\example.py
>>> F:\Project
"""
```
