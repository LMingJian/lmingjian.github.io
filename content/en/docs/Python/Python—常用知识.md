---
title: Python 常用知识
date: 2021-01-13
author: LM
---

## 1.python 的优点

- ① Python的定位是优雅、明确、简单，所以 Python 程序看上去总是简单易懂，初学者学 Python，不但入门容易，而且将来深入下去，可以编写那些非常非常复杂的程序。
- ② 开发效率非常高，Python 有非常强大的第三方库，基本上你想通过计算机实现任何功能，Python 官方库里都有相应的模块进行支持，直接下载调用后，在基础库的基础上再进行开发，大大降低开发周期，避免重复造轮子。
- ③ 高级语言——当你用 Python 语言编写程序的时候，你无需考虑诸如如何管理你的程序使用的内存一类的底层细节。
- ④ 可移植性——由于它的开源本质，Python 已经被移植在许多平台上（经过改动使它能够工作在不同平台上）。如果你小心地避免使用依赖于系统的特性，那么你的所有 Python 程序无需修改就几乎可以在市场上所有的系统平台上运行。
- ⑤ 可扩展性——你可以把你的部分程序用 C 或 C++ 编写，然后在你的 Python 程序中使用它们。
- ⑥ 可嵌入性——你可以把 Python 嵌入你的 C/C++ 程序，从而向你的程序用户提供脚本功能。

## 3.python的缺点

- ① 速度慢，Python 的运行速度相比C语言确实慢很多，跟 Java 相比也要慢一些，但其实在大多数情况下 Python 已经完全可以满足你对程序速度的要求，除非你要写对速度要求极高的搜索引擎等。
- ② 代码不能加密，因为 Python 是解释性语言，它的源码都是以明文形式存放的。
- ③ 线程不能利用多 CPU 运行，GIL即全局解释器锁（Global Interpreter Lock），是计算机程序设计语言解释器用于同步线程的工具，使得任何时刻仅有一个线程在执行，Python 的线程是操作系统的原生线程。在 Linux 上为 pthread，在 Windows 上为 Win thread，完全由操作系统调度线程的执行。一个 Python 解释器进程内有一条主线程，以及多条用户程序的执行线程。即使在多核 CPU 平台上，由于 GIL 的存在，所以会禁止多线程的并行执行。

## 4.Python 文件开头写入 /usr/bin/python 和 /usr/bin/env python 的区别

- `#!/usr/bin/python` 是告诉操作系统执行这个脚本的时候，调用 /usr/bin 下的 Python 解释器。
- `#!/usr/bin/env python` 这种用法是为了防止操作系统用户没有将 Python 装在默认的 /usr/bin 路径里。当系统看到这一行的时候，首先会到 env 设置里查找Python 的安装路径，再调用对应路径下的解释器程序完成操作。

## 5.变量

- ① 变量名只能是字母、数字或下划线的任意组合

- ② 变量名的第一个字符不能是数字

- ③ 以下关键字不能声明为变量名
  
  ```python
  ['and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'exec', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'not', 'or', 'pass', 'print', 'raise', 'return', 'try', 'while', 'with', 'yield']
  ```

## 6.常量

不能改变的量，变量名要大写

## 7.输出

### ① 标准输出

```python
print("ASDDF")
```

### ② 使用占位符输出

```python
name=input("name：")
age=int (input("age："))
Job=input("Job:")
salary=float (input("salary:"))
info="""
    -----------info of %s-----------
    name:%s
    age:%d
    Job:%s
    salary:%f
    -------------------------------
""" %(name,name,age,Job,salary)
print(info)
# 其中 %s 是占位符
```

### ③ 使用 format 输出

```python
name=input("name：")
age=int(input("age："))
job=input("Job:")
salary = float(input("salary:"))
info2='''
    =================info of {_name}====================
    name:{_name}
    age:{_age}
    job:{_job}
    salary:{_salary}
    ===================================================
'''.format(_name=name,
               _age=age,
               _job=job,
               _salary=salary)
print(info2)
# 使用 format 进行标准输出，利用 key 的形式
# 格式：.format(_name=name,_age=age,_job=job,_salary=salary)
```

