---
title: Python 输出自定义
date: 2023-11-16
author: LM
---

## 1.重写 print 实现自定义输出

在程序执行前，通过重写 print 的实现，使得后续程序执行 print 输出时，能自定义输出格式。

```python
import os,sys,time,io
import builtins as __builtin__

def print(*args, **kwargs):
    return __builtin__.print(time.strftime("%Y-%m-%d %H:%M:%S -----  ", time.localtime()) ,*args, **kwargs)

```

{{< details "参考文件" >}} 
1：[ Python 重写 print @CSDN ](https://blog.csdn.net/qq_39621009/article/details/122429010)
{{< /details >}}