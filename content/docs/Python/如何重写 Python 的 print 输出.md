---
title: 如何重写 Python 的 print 输出
date: 2024-12-25T16:10:43+08:00
author: LiangMingJian
---

# 代码实现

在程序执行前，通过重写 print，使得后续程序执行 print 输出时，能自定义输出格式。

```python
import os,sys,time,io
import builtins as __builtin__

def print(*args, **kwargs):
    return __builtin__.print(time.strftime("%Y-%m-%d %H:%M:%S -----  ", time.localtime()) ,*args, **kwargs)

```

{{< details "参考文件" >}} 
1：[ Python 重写 print @CSDN ](https://blog.csdn.net/qq_39621009/article/details/122429010)
{{< /details >}}
