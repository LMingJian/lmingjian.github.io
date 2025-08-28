---
title: 如何处理 Python 中的异常信息
date: 2024-12-25T16:08:52+08:00
author: LiangMingJian
---

# 使用 try ... except ... 捕获异常

在 Python 中，处理异常的标准方法就是使用 `try...except` 语句。

**可能产生异常的代码应该被 `try` 语句囊括，当出现异常时，程序会立即停止 `try` 语句中剩余代码的执行，并执行 `except` 语句中的代码。**

**在 `except` 中捕获的异常，可以通过 `as` 语句捕获为一个变量，用于在后续处理代码中打印。**

```python
try:
    pass
except BaseException as e:
    print(e)
else:
    pass
finally:
    pass
```

**可以通过多个 `except` 子句来处理多个异常，每个子句都可以处理一些特定的异常。**

```python
try:
    # 可能引发异常的代码
    pass
except ZeroDivisionError as e:
    # 处理特定异常
    print(f"捕获到异常: {e}")
except (TypeError, ValueError) as e:
    # 处理多个异常（使用元组包括需要捕获的异常）
    print(f"类型或值错误: {e}")
```

我们也可以在 `try ... except` 代码块中使用 `else` 和 `finally` 子句。

**在 `try` 子句完成而没有引发任何异常时，执行 `else` 子句。**

**在整个 `try…except` 完成之后运行 `finally` 子句（无论是否出现异常）。**

```python
def divide_six(number):
    try:
        formatted_number = int(number)
        result = 6/formatted_number
    except ValueError:
        print("This is a ValueError")
    else:
        print("Else")
    finally:
        print("Finally")
```

# 常见的异常种类

Python 主要有 5 种异常基类，这些基类都是其他异常的父亲，捕获这个相当于捕获其包括的所有异常。这 5 个基类具体有：

- `BaseException`：所有异常的基类，如果不知道该捕获什么异常，可以直接捕获这个。
- `Exception`：所有内置异常的基类，与 `BaseException` 不同，`Exception` 不会捕获系统退出类异常。
- `ArithmeticError`：所有算术相关异常的基类。
- `BufferError`：所有缓存区异常的基类。
- `LookupError`：所有字典，列表等键或索引无效的异常基类。

通常在编写代码时，不建议直接捕获基类，因为基类包括的错误类型太过于广泛，不利于代码调试。因此，在编写代码时，更建议捕获到具体的错误类型，比如常见的：

- `IndexError`：列表索引越界超出。
- `KeyError`：字典读取不到键。
- `KeyboardInterrupt`：用户按下中断键，Control + C 或 Delete。
- `TypeError`：类型异常。
- `ValueError`：非法的输入值。

更多以及详细的异常分类可以查看文档：[内置异常，Ver 3.10](https://link.zhihu.com/?target=https%3A//docs.python.org/zh-cn/3.10/library/exceptions.html)

# 主动抛出异常

**在 Python 中，程序使用 raise 关键字加异常类（例如 Exception，NameError）的方式来抛出异常。**

```python
>>> # Raise exceptions
>>> raise Exception
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
Exception

>>> raise NameError
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError
```

**可以通过使用异常类构造函数，在抛出时携带自定义异常信息。**

```python
>>> raise ValueError("You can't divide something with zero.")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: You can't divide something with zero.

>>> raise NameError("It's silly to make this mistake.")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: It's silly to make this mistake.
```

**可以通过继承异常基类自定义异常**

```python
import requests

class APIError(Exception):
    """接口异常"""
    def __init__(self, message="接口异常"):
        super().__init__(message)

code = requests.get('https://www.zhihu.com?123').status_code

if code == 200:
    print(code)
else:
    print(code)
    raise APIError
```

可以让自定义异常携带更多参数

```python
import requests

class APIError(Exception):
    """接口异常"""
    def __init__(self, status_code, message="接口异常"):
        super().__init__(f'{message}: {status_code}')

code = requests.get('https://www.zhihu.com?123').status_code

if code == 200:
    print(code)
else:
    raise APIError(code)
```

**可以继承自定义的异常**

```python
import requests

class APIError(Exception):
    """接口异常"""
    def __init__(self, message="接口异常"):
        super().__init__(message)

class Error403(APIError):
    """403"""
    def __init__(self, message="403"):
        super().__init__(message)

code = requests.get('https://www.zhihu.com?123').status_code

try:
    if code == 200:
        print(code)
    elif code == 403:
        print(code)
        raise Error403
except APIError:
    print('接口出问题了')
```
