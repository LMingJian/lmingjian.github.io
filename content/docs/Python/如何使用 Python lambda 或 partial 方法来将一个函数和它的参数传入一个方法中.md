---
title: 如何使用 Python lambda 或 partial 方法来将一个函数和它的参数传入一个方法中
date: 2025-11-10T11:57:50+08:00
author: LiangMingJian
---

# 需求

在使用 Python 完成诸如事件处理，程序回调，信号绑定等工作时，我们往往需要将一个函数作为参数传入目标方法中。

比如下面代码，将函数 `callback_func` 作为方法 `target_func` 的参数直接传入：

```python
def callback_func():  
    print('我是一个回调函数')  
  
def target_func(flag, callback):  
    if flag:  
        callback()  
  
if __name__ == '__main__':  
    target_func(True, callback_func)
    # 执行并打印：我是一个回调函数
```

在某些时候，上面的写法还不足以满足我们的需求。

比如，当我们编写的回调方法中也存在参数时，上面的写法在执行时会报错：

```python
def callback_func(number):  
    print(f'我是 {number} 回调函数')  
  
def target_func(flag, callback):  
    if flag:  
        callback()  
  
if __name__ == '__main__':  
    target_func(True, callback_func)
    # 执行然后报错：TypeError: callback_func() missing 1 required positional argument: 'number'
```

此时，要正确的给目标方法传入一个函数与它对应的参数，我们需要使用到 `lambda` 表达式或 `functools.partial` 方法。

# 使用 lambda 表达式

lambda 表达式可以用来创建匿名函数，在使用时，可以将表达式中带参数的函数转换为一个新的不带参数的函数，从而实现我们的需求。

```python
def callback_func(number):  
    print(f'我是 {number} 回调函数')  
  
def target_func(flag, callback):  
    if flag:  
        callback()  
  
if __name__ == '__main__':  
    target_func(True, lambda: callback_func(1))
    # 正常执行打印：我是 1 回调函数
```

**特别注意，在进行事件绑定时（PySide6 的按钮点击事件），禁止在循环内使用 lambda 函数来为多个按钮进行绑定。**

比如下述代码，通过 lambda 函数在一个循环内为多个按钮绑定事件，其执行结果会出现异常，**所有的按钮绑定事件所调用的 number 值都会是循环中最后一个迭代值**：

```python
...
for number, name in config:  
    func_button[number].setText(name)  
    func_button[number].clicked.connect(lambda: callback_func(number))
...
```

这是因为 lambda 的闭包延迟绑定‌机制。

事件函数在循环中创建时，只是创建了对变量 number 的引用，而不是在创建时立即捕获 number 的当前值。

因此，lambda 函数在定义时不会立即捕获变量 number 的值，而是在调用时才会查找 number 的值。又因为循环在结束后，number 会保留最后一次迭代的值。所以，在等到 lambda 函数调用时，使用的 number 值都会是最后一次迭代的值，同一个值。

**要解决这个问题，我们可以将 lambda 表达式替换为 partial 方法。**

# 使用 partial 方法

partial 方法是 Python 标准库 functools 中的一个实用函数工具，它可以固定一个函数和它的参数，然后生成一个新的不带参数可调用对象，从而实现我们的需求。

```python
from functools import partial  
  
def callback_func(number):  
    print(f'我是 {number} 回调函数')  
  
def target_func(flag, callback):  
    if flag:  
        callback()  
  
if __name__ == '__main__':  
    target_func(True, partial(callback_func, 1))  
    # 正常执行打印：我是 1 回调函数
```

当需要固定多个参数，或者固定关键字参数时，partial 方法可以这样使用：

```python
# 固定多个参数
partial(callback_func, 1, 2, 3)

# 固定关键字参数
partial(callback_func, a=1, b=2, c=3)

# 混合使用
partial(callback_func, 1, 2, key3=3)
```
