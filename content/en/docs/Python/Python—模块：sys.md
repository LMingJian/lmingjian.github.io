---
title: Python 模块：sys
date: 2021-03-24
author: LM
---

## 1.简介

sys 模块为用户提供了一些接口，用于访问 Python 解释器自身使用和维护的变量，以及与解释器进行深度的交互，但需要注意的是 sys 模块针对的是与 Python 解释器的操作，不是主机的操作系统。

## 2.使用：执行参数提供

argv ( argument value ) 是一个列表对象，存储在命令行调用 Python 脚本时提供的命令行参数。这个列表中第一个参数是被调用的脚本名称，也就是说，python 这个命令本身并没有被加入这个列表，这一点与 C 程序有所不同，C 程序读取命令行参数是从头开始的。

举例来说，在当前目录下新建一个 Python 文件 test.py，其内容为：

```python
import sys
print("The list of command line arguments:\n", sys.argv)
```

在命令行运行该脚本：

```bash
$ python test.py
The list of command line arguments:
['example.py'] 
```

加上几个参数运行，可以看到参数被获取并输出

```bash
$ python test.py arg1 arg2 arg3
The list of command line arguments:
['example.py', 'arg1', 'arg2', 'arg3']
```

## 3.使用：系统路径

sys.path 是一个由字符串组成的列表，其中的各个元素是 Python 搜索模块的路径，Python 在执行 import 时，会根据这个列表中的路径进行查找。

## 4.使用：输出到文件

可以将文件对象赋值给 sys.stdout，来让默认输出指向文件。

```bash
f_handler = open('out.log', 'w') 
sys.stdout = f_handler 
print('hello')
# 你无法在屏幕上看到 hello，因为它被写到 out.log 文件里了
```

{{< details "参考文件" >}} 
1：[ Python sys 模块详解   @轩辕御龙  ](https://zhuanlan.zhihu.com/p/150835014)
{{< /details >}}