---
title: Python 的装饰器
date: 2024-12-25T16:11:22+08:00
author: LiangMingJian
---

# 什么是装饰器

装饰器本质上是一个 Python 函数，它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，装饰器的返回值也是一个函数对象。

装饰器经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。

概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。

# 为什么要实现装饰器

这里已经有 3 个编写好的函数 a, b, c，各个函数的内容如下所示：

```python
def a():
    print('1')
    
def b():
    print('2')

def c():
    print('3')
```

这个时候，用户想让函数执行的时候打印它自己的名字，怎么解决？

正常的做法是给每个函数都加上一行代码，让它打印自己的名字，比如：

```python
 def a():
    print('1')
    print('我是函数 a')
```

但这就有问题了，如果函数很多，或者要加的功能不止一行代码，那给所有函数都这样操作是一件很复杂工作，同时在后续维护的时候也很麻烦（有点地方要修改，要一个一个函数去修改）。

这个时候，我们就可以使用装饰器，将功能抽象出来，然后给每个函数调用。

# 如何实现一个装饰器

我们来看下面的代码，这里的代码实现了一个简单的装饰器。

```python
def logging(func):
    def wrapper(*args, **kwargs):
        print(f"我是函数： {func.__name__}")
        return func(*args, **kwargs)
    return wrapper
```

这个代码里，我们直接将函数 func 作为变量传递。（Python 的函数像普通对象一样，能作为参数传递给其他函数，能被赋值给其他变量，能作为返回值，甚至能被定义在另外一个函数内）

然后用 wrapper 代替 func 的运行，`wrapper(*args, **kwargs)` 可以接收所有的参数，最后在 `return` 时传递回 func，正常执行 func 的逻辑。

在 `return` 返回前，我们就可以添加自己想要多加的操作，比如打印 func 的名字。

形如上面这种结构的函数，我们都可以称之为装饰器。

# 如何使用装饰器

装饰器的使用极其简单，只需在想要使用的函数上用 `@ + 装饰器名称` 即可。注意，装饰器要先于原函数定义，这样原函数在调用时才能找到目标。

```python
def logging(func):
    def wrapper(*args, **kwargs):
        print(f"我是函数： {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@logging
def a():
    print('a')

@logging
def b():
    print('b')

@logging
def c():
    print('c')

a()
b()
c()
```

# 如何实现一个带参数的装饰器

```python
def logging(date):
    def decorator(func):
        def wrapper(*args, **kwargs):
            print(f'我是参数： {date}')
            print(f"我是函数： {func.__name__}")
            return func(*args, **kwargs)
        return wrapper
    return decorator
```

上面的代码实现了一个支持传递参数的装饰器，它实际上是原有装饰器的再一个装饰器封装。

在这串代码中，`decorator(func)`可以看作是原本装饰器，`logging(date)`是用接收参数再一个装饰器，Python 能自动的发现这层结构，然后将参数传递到里层装饰器中。

使用：

```python3
@logging(1)
def a():
    print('a')

a()
```

# 装饰器的缺陷

从上面的描述中，我们知道装饰器是通过替代函数执行实现的，因此不难看出，装饰器的缺陷就是原函数的信息会在替代时丢失，丢失原函数的元数据。

```python
@logging
def a():
    print('a')

def b():
    print('b')

print(a.__name__)
print(b.__name__)
```

不难看出，在使用装饰器后，函数的 `__name __` 会被装饰器替代，由 `a` 变成了 `wrapper`。

那有没有办法解决呢？答案肯定是有的。

使用装饰器 `@functools.wraps(func)`，能将原函数的元数据复制到装饰器中。

```python
import functools

def logging(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"我是函数： {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@logging
def a():
    print('a')

def b():
    print('b')

print(a.__name__)
print(b.__name__)
```

可以看到，在使用 `@functools.wraps(func)` 后，函数的 `__name__` 正常了。

# 装饰器的执行顺序

在 Python 中，装饰器是支持多个使用的，多个装饰器的执行顺序是**从下往上**的，比如：

```python
@a
@b
@c
def func()
```

上面代码中，先执行 c，再执行 b，最后执行 a，其执行等效于 `func = a(b(c(func)))`。
