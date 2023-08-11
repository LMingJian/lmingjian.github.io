---
title: Python 异常信息处理
date: 2023-08-10
author: LM
---

## 1.异常处理的基本形式

在 Python 中，处理异常的标准方法就是使用 `try...except` 语句。可能产生异常的代码应该被 `try` 语句囊括，当出现异常时，程序会立即停止 `try` 语句中剩余代码的执行，并执行 `except` 语句中的代码。

```python
def divide_twelve(number):
    try:
        print(f"Result: {12/number}")
    except ZeroDivisionError:
        print("You can't divide 12 by zero.")
```

我们可以使用 `as` 将异常赋值给某个变量，并将其打印出来。

```python
def divide_twelve(number):
    try:
        print(f"Result: {12/number}")
    except ZeroDivisionError as ex:
        print(ex)
```

我们可以通过多个`except`子句来处理多个异常，每个子句都可以处理一些特定的异常。

```python
def divide_six(number):
    try:
        formatted_number = int(number)
        result = 6/formatted_number
    except ValueError:
        print("This is a ValueError")
    except ZeroDivisionError:
        print("This is a ZeroDivisionError")
```

我们也可以在 `try ... except` 代码块中使用 `else` 和 `finally` 子句，在 `try` 子句完成而没有引发任何异常时，执行 `else` 子句，在整个 `try…except` 完成之后运行 `finally` 子句（无论是否出现异常）。

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

## 2.抛出异常

在 Python 中，程序使用 raise 关键字加异常类（例如 Exception，NameError）的方式来抛出异常。

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
>>> raise ValueError()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError
```

我们可以通过使用异常类构造函数，来自定义异常信息。

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

{{< details "参考文件" >}} 
1：[ 如何在Python中处理异常以及抛出异常？@知乎 ](https://zhuanlan.zhihu.com/p/147564050)
{{< /details >}}

