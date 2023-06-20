---
title: Python 概念：Callable
date: 2021-11-25
author: LM
---

## 1.什么是 callable

一个可 callable 的对象是指可以被调用执行的对象，并且可以传入参数。用另一个简单的描述方式是，只要可以在一个对象的后面使用小括号来执行代码，那么这个对象就是 callable 对象，callable 对象的种类包括函数，类，类里的函数，实现了 `__call__` 方法的实例对象。

### a.函数

函数是 python 里的一等公民，函数是可调用对象，使用 callable 函数可以证明这一点。

```python
def test():
    print('ok')

print(callable(test))   # True
test()  # ok
```

### b.类

在其他编程语言里，类与函数可以说是两个完全不搭的东西，但在 python 里，都是可调用对象。

```python
class Stu(object):
    def __init__(self, name):
        self.name = name


print(callable(Stu))     # True
print(Stu('小明').name)   # 小明
```

### c.类里的方法

类里的方法也是用 def 定义的，本质上也是函数。使用 isfunction 函数可以判断一个对象是否是函数，run 方法也是可调用对象。

```python
from inspect import isfunction, ismethod


class Stu(object):
    def __init__(self, name):
        self.name = name

    def run(self):
        print('{name} is running'.format(name=self.name))

print(isfunction(Stu.run))     # True
stu = Stu("小明")
stu.run()        # 小明 is running
```

### d.实现了`__call__`方法的实例对象

当你执行 stu() 时，与调用一个函数有着完全一致的体验，如果不告诉你 stu 是一个类的实例对象，你还以为 stu 就是一个函数。

```python
class Stu(object):

    def __init__(self, name):
        self.name = name

    def __call__(self, *args, **kwargs):
        self.run()

    def run(self):
        print('{name} is running'.format(name=self.name))

stu = Stu('小明')
print(callable(stu))    # True
stu()                   # 小明 is running
```

{{< details "参考文件" >}} 
1：[ python 中的 callable 概念 @知乎](https://zhuanlan.zhihu.com/p/191419441)
{{< /details >}}