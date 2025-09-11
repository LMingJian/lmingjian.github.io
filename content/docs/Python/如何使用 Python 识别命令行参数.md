---
title: 如何使用 Python 识别命令行参数
date: 2024-12-25T16:12:50+08:00
author: LiangMingJian
---

# 需求

在我们编写好 Python 脚本，打包给用户使用时，往往可能想要提供一些特别参数，比如：-version，-all，-p 等，让用户在终端调用时通过命令行传入，然后实现一些特别的功能。

就像下述 Python 命令所示：

```python
>>> python --version
>>> Python 3.13.7
```

# 通过 argparse 模块实现

argparse 是 Python 中一个用于简单设计命令行接口的模块。argparse 模块支持通过简单的语句自动识别传入的命令行接口参数，支持自动生成帮助和用法消息文本，还支持在用户传入无效参数时发出错误提示。

argparse 的使用基于以下三个组件：

- `argparse.ArgumentParser`：命令行参数解析器实例对象。
- `ArgumentParser.add_argument()`：命令行参数设置方法。
- `ArgumentParser.parse_args()`：命令行参数获取方法。

基本的示例代码如下：

```python
import argparse  
  
# 实例化命令行参数解析器
parser = argparse.ArgumentParser()  

# 设置命令行参数
parser.add_argument('-input', help='input file path')  
# 使用：example.py --input exp.js 
# 可以使用 parser.print_help() 获取帮助文档

# 获取命令行参数
args = parser.parse_args()  
print(args.input)
# 输出 exp.js
```

# ArgumentParser 对象

在 ArgumentParser 对象中，支持传入一些参数来设置命令行帮助信息中的一些默认行为，包括程序名称，命令用法，描述，备注，错误命令信息等。

ArgumentParser 对象设置的帮助信息会在使用 `-h` 或 `--help` 参数，以及在代码中调用 `print_help()` 方法时显示。

```python
>>> example -h
>>> example --help
```

## prog（程序名）

帮助信息中的程序名称。一般情况下，prog 会由 Python 自行计算，用户无需设置。

特别的，该值可在帮助消息中以 `%(prog)s` 进行调用。

**自动设置**

```python
import argparse  

parser = argparse.ArgumentParser()  
parser.add_argument('--input', help='%(prog)s input file path')  
parser.print_help()
"""
usage: example.py [-h] [--input INPUT]

optional arguments:
  -h, --help     show this help message and exit
  --input INPUT  example.py input file path
"""
```

**手动设置**

```python
import argparse 

parser = argparse.ArgumentParser(prog='myprogram')  
parser.add_argument('--input', help='%(prog)s input file path')  
parser.print_help()
"""
usage: myprogram [-h] [--input INPUT]

optional arguments:
  -h, --help     show this help message and exit
  --input INPUT  myprogram input file path
"""
```

## usage（用法）

帮助信息中的用法描述。一般情况下，usage 会由 Python 自行计算，用户无需设置。

```python
import argparse  
  
parser = argparse.ArgumentParser(usage='%(prog)s [options]')  
parser.print_help()
"""
usage: example.py [options]

optional arguments:
  -h, --help  show this help message and exit
"""
```

## description（描述）与 epilog（备注）

description 用以在帮助信息的 usage 栏与 optional arguments 栏间显示描述。

epilog 用以在帮助信息最后一栏显示备注。

```python
import argparse  
  
parser = argparse.ArgumentParser(description='我是开头描述',  
                                 epilog="我是结尾备注")  
parser.print_help()
"""
usage: example.py [-h]

我是开头描述

optional arguments:
  -h, --help  show this help message and exit

我是结尾备注
"""
```

## formatter_class（格式）

formatter_class 用以设置帮助文档中文本格式（主要是 description 和 epilog）。

**不传入，不保留原文本的格式信息**

```python
import argparse  
  
# 不设置格式时自动变成一行
parser = argparse.ArgumentParser(  
    description='''我是第 1 行\n我是第 2 行\n我是第 3 行''')
parser.print_help()
"""
usage: example.py [-h]

我是第 1 行 我是第 2 行 我是第 3 行

optional arguments:
  -h, --help  show this help message and exit
"""
```

**argparse.RawDescriptionHelpFormatter，保留原文本格式信息，但只针对 usage，description 和 epilog 部分**

```python
import argparse

# RawDescriptionHelpFormatter
parser = argparse.ArgumentParser(  
    formatter_class=argparse.RawDescriptionHelpFormatter,  
    description='''我是第 1 行\n我是第 2 行\n我是第 3 行''') 
parser.add_argument('--input', help='------\ninput file path')
parser.print_help()
"""
usage: example.py [-h] [--input INPUT]

我是第 1 行
我是第 2 行
我是第 3 行

optional arguments:
  -h, --help     show this help message and exit
  --input INPUT  ------ input file path
"""
```

**argparse.RawTextHelpFormatter，保留原文本格式信息，针对所有帮助文本**

```python
import argparse

# RawTextHelpFormatter
parser = argparse.ArgumentParser(  
    formatter_class=argparse.RawTextHelpFormatter,  
    description='''我是第 1 行\n我是第 2 行\n我是第 3 行''')  
parser.add_argument('--input', help='------\ninput file path')  
parser.print_help()
"""
usage: example.py [-h] [--input INPUT]

我是第 1 行
我是第 2 行
我是第 3 行

optional arguments:
  -h, --help     show this help message and exit
  --input INPUT  ------
                 input file path
"""
```

**argparse.ArgumentDefaultsHelpFormatter，给参数添加默认值信息**

```python
import argparse  

# ArgumentDefaultsHelpFormatter
parser = argparse.ArgumentParser(  
    formatter_class=argparse.ArgumentDefaultsHelpFormatter,  
    description='''我是第 1 行\n我是第 2 行\n我是第 3 行''')  
parser.add_argument('--number', help='------\ninput number', default=10)  
parser.print_help()
"""
usage: example.py [-h] [--number NUMBER]

我是第 1 行 我是第 2 行 我是第 3 行

optional arguments:
  -h, --help       show this help message and exit
  --number NUMBER  ------ input number (default: 10)
"""
```

**argparse.MetavarTypeHelpFormatter，给参数添加类型信息**

```python
import argparse  

# MetavarTypeHelpFormatter
parser = argparse.ArgumentParser(  
    formatter_class=argparse.MetavarTypeHelpFormatter,  
    description='''我是第 1 行\n我是第 2 行\n我是第 3 行''')  
parser.add_argument('--number', help='------\ninput number', type=int)  
parser.print_help()
"""
usage: example.py [-h] [--number int]

我是第 1 行 我是第 2 行 我是第 3 行

optional arguments:
  -h, --help    show this help message and exit
  --number int  ------ input number
"""
```

**结合使用，一次性使用多个格式**

```python
import argparse  

# 通过继承结合多个格式
class MyFormater(  
    argparse.RawTextHelpFormatter,  
    argparse.ArgumentDefaultsHelpFormatter,  
    argparse.MetavarTypeHelpFormatter):  
    pass  

# 结合使用
parser = argparse.ArgumentParser(  
    formatter_class=MyFormater,  
    description='''我是第 1 行\n我是第 2 行\n我是第 3 行''')  
parser.add_argument('--number', help='------\ninput number', 
					type=int, default=10)  
parser.print_help()
"""
usage: example.py [-h] [--number int]

我是第 1 行
我是第 2 行
我是第 3 行

optional arguments:
  -h, --help    show this help message and exit
  --number int  ------
                input number (default: 10)
"""
```

## prefix_chars（前缀）

设置命令行选项识别的前缀，默认值为 `-`，识别以 `-f/--foo` 的命令。

通过设置为 `-+`，可以识别以 `-f/--foo` 和 `+f/++foo` 的命令。

特别的，如果设置为 `+`，会导致传入 `-f/--foo` 报错，因为没有这个前缀匹配。

> 更多的 ArgumentParser 对象参数请查阅：[ ArgumentParser 对象文档 ](https://docs.python.org/zh-cn/3.13/library/argparse.html#argumentparser-objects)

# add_argument 方法

add_argument 方法用以告知 ArgumentParser 对象可以接收什么样的参数，这些参数的格式是什么样的。没有在 add_argument 方法中标记的参数，在传入时会报错。

## name or flags（参数名称）

add_argument 支持设置的参数包括可选参数与位置参数（位置参数是必传的）。

```python
import argparse  
  
parser = argparse.ArgumentParser() 
# 设置可选参数，通常 - 简写，-- 全写
parser.add_argument('-f', '--foo')
# 设置位置参数
parser.add_argument('bar')  
parser.print_help()
"""
usage: example.py [-h] [-f FOO] bar

positional arguments:
  bar

optional arguments:
  -h, --help         show this help message and exit
  -f FOO, --foo FOO
"""
```

## action（行为）

设置传入命令行参数的行为。

**store：默认的行为，保存参数的值**

```python
parser.add_argument('--foo')
# 使用：--foo 10
# 可以读到属性：foo=10
```

**store_const：存储 const 的值，相当于这个参数只是一个标志。此外，使用 store_true 和 store_false 可以用来存储 True 和 False 值。**

```python
parser.add_argument('--foo', action='store_const', const=10)
# 使用：--foo
# 可以读取到属性：foo=10
# 注意，如果使用为 --foo 10，则程序会报错

parser.add_argument('--boo', action='store_true')
# 使用：--boo
# 可以读到属性：boo=True
```

**append：将参数存储为一个列表，适用于多次指定的参数值。**

```python
parser.add_argument('--foo', action='append')
# 使用：--foo 10 --foo 20
# 可以读到属性：foo=['10', '20']
```

**append_const：与 store_const 类似，将参数以对应的 const 值存储到一个列表。此外，如果使用 dest 指定参数属性名，那么相同属性名添加的 const 值会在同一个列表。**

```python
parser.add_argument('--foo', action='append_const', const=10)
# 使用：--foo
# 可以读到属性：foo=['10']

parser.add_argument('--str', dest='types', 
					action='append_const', const='str')
parser.add_argument('--int', dest='types', 
					action='append_const', const='int')
# 使用：--str --int
# 可以读到属性：types=['str', 'int']
```

**extend：将多个参数存储到一个列表。extend 需要与 nargs 关键字结合使用，通过限定 `+` 或 `*`，允许命令行传入多个参数。**

> 需要 Python 版本 3.8

```python
parser.add_argument("--foo", action="extend", nargs="+")
# 使用：--foo 10 20 30 40 50
# 可以读到属性：foo=['10', '20', '30', '40', '50']
```

**count：统计一个关键字出现的次数，并记录。**

**help：输出帮助文档。**

**version：输出版本号。需要结合关键字参数 version 使用。**

```python
parser.add_argument('--version', action='version', version='example 2.0')
# 使用：--version
# 输出：example 2.0
```

## nargs（关联参数数量）

默认情况下，一个命令行参数后面跟一个值，但通过 nargs 的设置，一个命令行参数后面可以关联跟随多个值。

**N，一个整数，默认值 1，命令行参数后面可以跟随 N 个值。**

```python
parser.add_argument('--foo', nargs=1)
parser.add_argument('--bar', nargs=2)
# 使用 --foo 1 --bar 1 2
# 可以读到：foo=['1'], bar=['1', '2']
```

**?，命令行参数后面只可以跟随 0 个或 1 个值。特别的，当不传入命令行参数时，使用 default 值，传入参数但不传值时，使用 const 值。**

```python
parser.add_argument('--foo', nargs='?', const='c', default='d')
# 使用 --foo 1，读取到 foo='1'
# 使用 --foo，读取到 foo='c'
# 不使用，读取到 foo='d'
```

**\*，命令行参数后面可以跟随 0 个或多个值，多个值将聚集到一个列表中。**

```python
parser.add_argument('--foo', nargs='*')
# 使用 --foo，可以读取到 foo=[]
# 使用 --foo 1 2 3，可以读取到 foo=['1', '2', '3']
```

**+，命令行参数后面可以跟随多个值，多个值将聚集到一个列表中，不能为空。**

```python
parser.add_argument('--foo', nargs='+')
# 使用 --foo，失败
# 使用 --foo 1 2 3，可以读取到 foo=['1', '2', '3']
```

## const（常值）

用以保存不从命令行读取，但又调用了命令行参数的值。

## default（默认值）

用以保存不传入命令行参数时，命令行参数使用的值。

## type（类型）

用以标记传入的数值以何种方式解析，常用的普通内置类型和函数都可以被使用。

```python
parser.add_argument('count', type=int)       # 以整型存储
parser.add_argument('distance', type=float)  # 以浮点型存储
# 以路径对象存储
parser.add_argument('datapath', type=pathlib.Path)
# 以文件类型存储，这个时候往往传入的是文件路径，然后将打开后的文件对象进行存储
parser.add_argument('dest_file', type=argparse.FileType('r', encoding='UTF-8'))
```

> [argparse.FileType](https://docs.python.org/zh-cn/3.13/library/argparse.html#filetype-objects)

## choices（限定选择）

用以限定传入的命令行参数值只能在给定范围内接收。

```python
parser.add_argument('--foo', choices=['1', '2', '3'])
# 使用：--foo 1，可以读取到 foo='1'
# 使用：--foo 4，使用失败
```

## required（强制必选）

用以将某个可选参数强制为必选参数，但不改变结构。

```python
parser.add_argument('--foo', required=True)
# 必须传入，不能置空或不填
```

## help（帮助信息）

用以在使用 `-h` 或 `--help` 时打印参数的提示信息。

```python
parser.add_argument('--foo', help="我是提示")
"""
usage: example.py [-h] [--foo FOO]

options:
  -h, --help  show this help message and exit
  --foo FOO   我是提示
"""
```

## dest（属性名）

用以标记属性的名称，默认是 name or flags 的值。

## metavar（指定引用标识）

在帮助信息打印时，程序往往会将属性名 dest 的大写作为引用标识，使用 metavar 可以将默认的标识进行替换。

```python
# 默认的标识 FOO
parser.add_argument('--foo')
"""
usage: example.py [-h] [--foo FOO]

options:
  -h, --help  show this help message and exit
  --foo FOO
"""

# 替换的标识 ABC
parser.add_argument('--foo', metavar='ABC')
"""
usage: example.py [-h] [--foo ABC]

options:
  -h, --help  show this help message and exit
  --foo ABC
"""
```

## deprecated（废弃信息）

用以告诉用户这个命令将在未来废弃。

> 需要 Python 版本 3.13

```python
parser.add_argument('--foo', deprecated=True)
# 在使用这个命令时会提示：warning: option '--foo' is deprecated
```

> 详细参数介绍请查看：[ add_argument 方法 ](https://docs.python.org/zh-cn/3.13/library/argparse.html#the-add-argument-method)

# parse_args 方法

parse_args 方法用以获取从命令行中读取的所有值。在读取成功后，parse_args 会返回一个 Namespace 命名空间对象，开发者能通过点式属性名直接读取对应的参数值。

```python
import argparse  
  
parser = argparse.ArgumentParser()  
parser.add_argument('--foo')  

# 读取数据
data = parser.parse_args()  
print(data.foo)
```

————————————

> [ argparse --- 用于命令行选项、参数和子命令的解析器 ](https://docs.python.org/zh-cn/3.13/library/argparse.html)
