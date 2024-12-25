---
title: Python 中的 Callable 是什么
date: 2024-12-25T16:11:15+08:00
author: LiangMingJian
---

# 什么是 Callable

Callable 是指一个支持传入参数，同时可被调用执行的对象。换一个说法就是，只要可以在一个对象的后面使用小括号来执行代码，那么这个对象就是一个 Callable 对象。

Python 中的 Callable 对象包括：函数，类，类里的函数，以及实现了 `__call__` 方法的实例对象。

## 函数

函数是 python 里的一等公民，函数是可调用对象，使用 Callable 函数可以证明这一点。

```python
def test():
    print('ok')

print(callable(test))   # True
test()  # ok
```

## 类

在其他编程语言里，类与函数可以说是两个完全不搭的东西，但在 python 里，都是可调用对象。

```python
class Stu(object):
    def __init__(self, name):
        self.name = name


print(callable(Stu))     # True
print(Stu('小明').name)   # 小明
```

## 类里的方法

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

## 实现了_\_call__方法的实例对象

当一个类具有 `__call__` 方法时，这个类就支持直接执行，而不需要实例化，比如下述代码中的 `stu()` ，在使用时与调用一个函数有着完全一致的体验。

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
