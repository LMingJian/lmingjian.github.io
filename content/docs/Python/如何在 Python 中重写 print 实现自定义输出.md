---
title: 如何在 Python 中重写 print 实现自定义输出
date: 2024-12-25T16:10:43+08:00
author: LiangMingJian
---

# 需求

在使用 print 打印一些数据的时候，可能想要这个输出携带时间数据，此时便可以通过简单重写 print 方法来实现。

注意，正确的做法是使用模块 logging，而不是重写内置函数，这里只是提供一个思路。

> 重写是编程中对现有功能或代码进行重新实现的一种操作。 通过重写可以实现：原有功能的优化，覆盖功能原有的逻辑，扩展或者修改原有的功能。

# 实现

## 给输出内容添加时间信息

```python
import time
import builtins as __builtin__

def print(*args, **kwargs):  # noqa
    return __builtin__.print(
        time.strftime("%Y-%m-%d %H:%M:%S -----  ", time.localtime()),
        *args,
        **kwargs)

print('I Love Python')
# 输出：2025-08-13 09:47:42 -----    I Love Python
```

## 置空所有 print 输出

```python
import builtins as __builtin__

def print(*args, **kwargs):  # noqa
    return __builtin__.print()

print('I Love Python')
# 输出为空
```

## 输出到控制台的同时写入文件

```python
import builtins as __builtin__

file = open('log.txt', 'a+', encoding='utf-8')

def print(*args, **kwargs):  # noqa
    sep = kwargs.get('sep', ' ')
    end = kwargs.get('end', '\n')
    message = sep.join(str(arg) for arg in args)
    file.write(message+end)  # 控制台输出时会自动回车，但文件里面不会，所以要手动加入
    return __builtin__.print(message)

print('I Love Python')
print('I Love C')
print('I Love C++')
file.close()
# 输出到控制台同时，输出到文件
```
