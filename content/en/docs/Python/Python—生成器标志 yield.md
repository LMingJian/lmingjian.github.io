---
title: Python yield 的使用
date: 2021-01-17
author: LM
---

## 1.生成器（generator）

带有 yield 的函数在 Python 中被称为 generator（生成器），而不是一个函数。generator 隐性带有一个 next 函数，在正常运行时，程序会在 yield 处停止，当程序调用 next 时，才会继续执行。

```python
def foo():
    print("starting...")
    while True:
        yield 5
        res = yield 4
        print("res:",res)
g = foo()
print(next(g))
print(next(g))
print(next(g))
```

## 2.为什么需要 yield

在这个循环`for i in range(1000): pass`中，由于 range 会生成一个列表，因此程序占用的内存会随着 range 参数的增大而增大，造成资源的浪费。而使用 yield 生成 iterable 迭代对象，则不会出现这个问题。程序在每次迭代返回下一个数值，不会生成列表，因此内存空间占用很小。generator 就是这样一个 iterable 对象。
