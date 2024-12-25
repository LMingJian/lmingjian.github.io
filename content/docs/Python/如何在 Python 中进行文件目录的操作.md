---
title: 如何在 Python 中进行文件目录的操作
date: 2024-12-25T16:10:30+08:00
author: LiangMingJian
---

# 需求

在编写脚本时，有时需要调取本地其他文件，或者创建文件夹保存文件。

# 使用`os.listdir() `返回文件列表

`os.listdir()` 方法用于返回指定的文件夹包含的文件或文件夹的名字的列表，但并不包括隐藏文件如 . 或 .. 开头的文件。

```python
import os, sys

path = "/var/www/html/"
dirs = os.listdir(path)
# path -- 需要列出的目录路径
# 返回指定路径下的文件和文件夹列表

for file in dirs:
    print(file)
```

# 使用`os.path.exists()`判断文件夹是否存在

```python
import os

path = "/var/www/html/"
# path -- 需要列出的目录路径

if os.path.exists(path):
    pass
```

# 使用`os.path.abspath()`返回文件绝对路径

```python
path1 = os.path.abspath(__file__)
print("path1:{}".format(path1))
```

# 使用`os.path.dirname()`获取当前文件的目录

```python
path2 = os.path.dirname(__file__)
print("path2:{}".format(path2))
```

# 使用`os.mkdir()`来创建文件夹

```python
import os

path="/var/www/html/ABC"
isExists=os.path.exists(path)

if not isExists:
    os.mkdir(path)
    print('创建成功')
else:
    print('目录已存在')
```

{{< details "参考文件" >}} 
1：[ os文档  @Python文档  ](https://docs.python.org/zh-cn/3/library/os.html)
{{< /details >}}
