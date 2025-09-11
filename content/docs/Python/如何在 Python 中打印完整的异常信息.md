---
title: 如何在 Python 中打印完整的异常信息
date: 2024-12-25T16:10:26+08:00
author: LiangMingJian
---

# 需求

一般情况下，我们可以通过 `try ... except .. as e` 语句，将异常存储于变量 e 中，然后进行打印。但这里有个问题，如果直接打印变量 e，打印结果中只会展示异常的名称，而不能展示完整的异常信息，如下述代码所示：

```python
# noinspection PyBroadException  
try:  
    1 / 0  
except Exception as e:  
    print(e)
# 输出：division by zero
```

因此存在需求，要求在脚本运行时打印部分完整的异常信息，而不只是异常名。

# 使用模块 traceback 打印完整异常

traceback 模块提供一些提取、格式化和打印 Python 程序栈回溯信息方法，通过 traceback 模块能更好的捕获异常，提高调试效率。

[ traceback --- 打印或读取栈回溯信息 官方文档 ](https://docs.python.org/zh-cn/3.13/library/traceback.html)

## 打印完整的异常信息

```python
import traceback  
  
# noinspection PyBroadException  
try:  
    1 / 0  
except Exception:  
    traceback.print_exc()
```

如下述，输出结果不仅包含错误类型，还包含错误所在的语句信息：

```python
Traceback (most recent call last):
  File "F:\MyPython\PythonProjectExample\example.py", line 5, in <module>
    1 / 0
ZeroDivisionError: division by zero
```

**特别的，也可以将捕获的异常，通过 traceback 来打印完整的信息**

```python
import traceback  
  
# noinspection PyBroadException  
try:  
    1 / 0  
except ZeroDivisionError as exc1:  
    traceback.print_exception(exc1)  
except Exception as exc2:  
    print(exc2)
```

结果同样和上述打印完整的异常信息时一样。

**支持将异常信息存储为字符串**

```python
import traceback  
  
# noinspection PyBroadException  
try:  
    1 / 0  
except Exception:  
    error = traceback.format_exc()  
    print('_' * 8)  
    print(error.strip())  
    print('_' * 8)  
    print(type(error))
```

输出结果如下：

```python
________
Traceback (most recent call last):
  File "F:\MyPython\PythonProjectExample\example.py", line 5, in <module>
    1 / 0
ZeroDivisionError: division by zero
________
<class 'str'>
```

## 打印栈跟踪信息

```python
import traceback  
  
def func1():  
    func2()  
    print('*' * 8)  
  
def func2():  
    traceback.print_stack()  
    print('_' * 8)  
  
func1()
```

正如下面输出，执行函数 `func1()` 到 `traceback.print_stack()` 所经过的所有栈信息都被成功打印：

```python
  File "F:\MyPython\PythonProjectExample\example.py", line 11, in <module>
    func1()
  File "F:\MyPython\PythonProjectExample\example.py", line 4, in func1
    func2()
  File "F:\MyPython\PythonProjectExample\example.py", line 8, in func2
    traceback.print_stack()
________
********
```

**支持将栈跟踪信息存储为列表**

```python
import traceback  
  
def func1():  
    func2()  
    print('*' * 8)  
  
def func2():  
    error = traceback.format_stack()  
    print(error)  
    print(type(error))  
    print('_' * 8)  
  
func1()
```

输出结果如下：

```python
['  File "F:\\MyPython\\PythonProjectExample\\example.py", line 13, in <module>\n    func1()\n', '  File "F:\\MyPython\\PythonProjectExample\\example.py", line 4, in func1\n    func2()\n', '  File "F:\\MyPython\\PythonProjectExample\\example.py", line 8, in func2\n    error = traceback.format_stack()\n']
<class 'list'>
________
********
```
