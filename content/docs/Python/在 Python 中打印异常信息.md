---
title: 如何在 Python 中打印异常信息
date: 2024-12-25T16:10:26+08:00
author: LiangMingJian
---

# 需求

在脚本运行时需要打印部分错误信息。

# 直接打印错误

```python
# noinspection PyBroadException (pycharm 警告注释)
try:
    pass
except KeyboardInterrupt:
    print("quit") 
except Exception as ex:
    print("出现如下异常%s"%ex)
#---------------------------------------------------
try:
    2/0
except Exception as e:
    print(e)
```

# 用 traceback 模块打印

上述方式看不到具体错误的信息，如行数，不便于调试的时候定位，因此可以使用 traceback 模块：

```python
import traceback
try:
    2/0
except Exception as e:
    traceback.print_exc()
```

结果为：

```python
Traceback (most recent call last):
 File "c:\Users\Administrator\Desktop\test1.py", line 3, in <module>
 2/0
ZeroDivisionError: division by zero
```
