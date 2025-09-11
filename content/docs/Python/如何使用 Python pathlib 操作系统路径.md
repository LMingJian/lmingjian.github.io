---
title: 如何使用 Python pathlib 操作系统路径
date: 2025-08-19T16:16:36+08:00
author: LiangMingJian
---

# 前言

在 Python 中，通常可以使用 os.path 来操作系统路径，但在版本 V3.4 后，Python 提供了一个更为方便的模块 pathlib 来供用户操作系统路径。

pathlib 与 os.path 最大的不同是，pathlib 提供的是一个具备多种功能的路径类，而 os.path 提供的是路径字符串。

> [ pathlib --- 面向对象的文件系统路径 ](https://docs.python.org/zh-cn/3.13/library/pathlib.html)

# 纯路径 PurePath

在使用 pathlib 时，提供两种使用方式，一种是纯路径 PurePath，一种是具体路径 Path。

纯路径 PurePath 提供一种不实际访问文件系统的路径处理操作。

## PurePath 的实例化

PurePath 实例化时支持传入单个或多个字符串，PurePath 会自动识别并拼接成符合规格的路径对象。（当不传入数据时，PurePath 默认返回当前路径）

```python
from pathlib import PurePath  
  
path = PurePath()  
print(path)
# 输出：.（当前的相对路径）

path = PurePath('date.py')  
print(path)
# 输出：date.py

path = PurePath('test', 'html/base', 'date.py')  
print(path)
# 输出：test\html\base\date.py（自动拼接）
```

需要注意，在拼接路径时，如果存在多个根路径，即以 `/` 开头的路径时，PurePath 会忽略最后一个根路径前的所有路径。

特别的，如果是 Windows 的路径，则盘符 `c:` 不会被忽略，盘符后面如果跟着路径则路径单独被忽略，如果存在多个盘符，则同样保留最后一个盘符。

```python
from pathlib import PurePath  
  
path = PurePath('/test', 'html', '/date', '/base', 'example.py')  
print(path)
# 输出：\base\example.py（忽略最后一个根路径 /base 前的所有）

path = PurePath('/test', 'html', 'C:/Windows', '/base', 'example.py')  
print(path)
# 输出：C:\base\example.py（盘符和最后一个根路径保留）

path = PurePath('C:/Windows', 'D:/', 'example.py')  
print(path)
# 输出：D:\example.py
```

## PurePath 的运算

PurePath 支持使用运算符 `/` 来创建子路径，提供如 `os.path.join()` 类似的功能。运算对象支持字符串，也支持 PurePath 对象。

同样的，如果运算的内容存在根路径或盘符，则原有路径会重置。

```python
from pathlib import PurePath

path = PurePath('/etc')  
print(path / 'date' / 'example.py')
# 输出：\etc\date\example.py

path = PurePath('/etc')  
print(path / '/date' / 'example.py')
# 输出：\date\example.py

path = PurePath('/etc')  
print(path / 'C:/date' / 'example.py')
# 输出：C:\date\example.py

path1 = PurePath('C:/date/test')  
path2 = PurePath('test/example.tar.gz')  
print(path1 / path2)
# 输出：C:\date\test\test\example.tar.gz
```

## PurePath 的方法

**PurePath.parent 返回父路径**

**PurePath.parents 返回父路径序列**

```python
from pathlib import PurePath  
  
path = PurePath('C:/date/test/example.py')  
print(path.parent)
# 输出：C:\date\test

path = PurePath('C:/date/test/example.py')  
for each in path.parents:  
    print(each)
# 输出：C:\date\test、C:\date、C:\
```

**PurePath.name 返回最后的路径名**

**PurePath.with_name(name) 替换最后的路径名**

```python
from pathlib import PurePath  
  
path = PurePath('C:/date/test/example.py')  
print(path.name)  
# 输出：example.py
  
path = PurePath('C:/date/test')  
print(path.name)
# 输出：test

print(path)  
print(path.with_name('abc'))
# 输出：C:\date\test 和 C:\date\abc
```

**PurePath.suffix 返回最后路径中文件的扩展名**

**PurePath.suffixes 返回最后路径中文件的扩展名序列**

**PurePath.with_suffix(suffix) 替换扩展名，如果不存在扩展名则添加**

```python
from pathlib import PurePath  
  
path = PurePath('C:/date/test/example.py')  
print(path.suffix) 
# 输出：.py
  
path = PurePath('C:/date/test/example.tar.gz')  
print(path.suffix)  
# 输出：.gz
  
path = PurePath('C:/date/test')  
print(path.suffix)
# 输出为空

path = PurePath('C:/date/test/example.tar.gz')  
print(path.suffixes)  
# 输出：['.tar', '.gz']
  
path = PurePath('C:/date/test')  
print(path.suffixes)
# 输出：[]

print(path)  
print(path.with_suffix('.bz'))
# 输出：C:\date\test 和 C:\date\test.bz
```

**PurePath.stem 返回最后的文件名，并去除后缀**

**PurePath.with_stem(stem) 替换文件名，不包括后缀**

```python
from pathlib import PurePath 

path = PurePath('C:/date/test/example.py')  
print(path.stem)
# 输出：example
  
path = PurePath('C:/date/test/example.tar.gz')  
print(path.stem) 
# 输出：example.tar

print(path)  
print(path.with_stem('bbs'))
# 输出：C:\date\test\example.tar.gz
# 输出：C:\date\test\bbs.gz
```

# 具体路径 Path

具体路径 Path 是纯路径 PurePath 的子类。除了支持后者提供的操作之外，还提供了对路径对象的系统调用。

## Path 的实例化

与 PurePath 一样，Path 的实例化同样支持传入单个路径与多个路径，同样支持拼接使用。

```python
from pathlib import Path  
  
path = Path()  
print(path)  
# 输出：.（当前的相对路径）  
  
path = Path('date.py')  
print(path)  
# 输出：date.py  
  
path = Path('test', 'html/base', 'date.py')  
print(path)  
# 输出：test\html\base\date.py（自动拼接）
```

## Path 的运算

与 PurePath 一样。

## Path 的方法

**`Path.home()` 返回用户的 Home 目录**

```python
from pathlib import Path  

print(Path.home())
# 输出：C:\Users\Admin
# 对于 Linux 这里是 /home
```

**`Path.cwd()` 返回当前文件夹的绝对路径**

```python
from pathlib import Path  

print(Path.cwd())
# 输出：F:\MyPython\PythonProjectExample
```

**`Path.absolute()` 返回指定对象的绝对路径**

**`Path.resolve()` 返回指定对象的绝对路径，同时会解析符号链接，如 `..`**

```python
from pathlib import Path  
  
path = Path('example.py')  
print(path.absolute())
# 输出：F:\MyPython\PythonProjectExample\example.py

path = Path('..\example.py')  
print(path.absolute())  
print(path.resolve())
# 输出：F:\MyPython\PythonProjectExample\..\example.py
# 输出：F:\MyPython\example.py
```

**`Path.stat()` 返回 `os.stat_result` 对象，可用于获取文件信息**

支持的属性请查看：[ os.stat_result 文档 ](https://docs.python.org/zh-cn/3.13/library/os.html#os.stat_result)

```python
from pathlib import Path  
  
path = Path('example.py')  
file_stst = path.stat()  
print(f'文件大小： {file_stst.st_size} B')
# 输出：文件大小： 127 B
```

**`Path.exists()` 判断指示路径是否存在**

```python
from pathlib import Path 

path = Path('example.py')  
print(path.exists())
# 输出：True
```

**`Path.is_file(), Path.is_dir()` 判断文件是否是文件或者文件夹**

```python
from pathlib import Path 

path = Path('example.py')  
print(path.is_file())  
print(path.is_dir())
# 输出：True
# 输出：False
```

**`Path.open(mode='r', buffering=-1, encoding=None, errors=None, newline=None)` 像 `open()` 一样打开文件**

```python
from pathlib import Path  
  
path = Path('example.py')  
with path.open('r') as f:  
    print(f.read())
```

**`Path.iterdir()` 返回一个包含指向目录下所有对象的序列**

```python
from pathlib import Path  
  
path = Path('.')  
for each in path.iterdir():  
    print(each)
# 返回当前文件夹中所有的文件夹和文件
```

**`Path.glob(pattern)` 返回指向目录中所有与通配符 pattern 匹配的文件**

支持的模式语言请查看：[ 模式语言文档 ](https://docs.python.org/zh-cn/3.13/library/pathlib.html#pathlib-pattern-language)

```python
from pathlib import Path  
  
path = Path('.')  
for each in path.glob('*.py'):  
    print(each)
# 只返回当前文件夹中所有以 .py 结尾的文件
```

**`Path.touch(mode=0o666, exist_ok=True)` 创建一个文件**

```python
from pathlib import Path  
  
path = Path('test/null.py')  
path.touch()
# 在指定的路径创建一个 null.py 文件
```

**`Path.mkdir(mode=0o777, parents=False, exist_ok=False)` 创建一个文件夹**

```python
from pathlib import Path  
  
path = Path('exp')  
path.mkdir()
# 在指定路径创建一个文件夹

path = Path('abc/exp')  
path.mkdir(parents=True)
# 当 parents=True 时，嵌套文件夹会连父文件夹一起生成
```

**`Path.rename(target)` 重命名指定路径的文件和文件夹为 target**

```python
from pathlib import Path  
  
path = Path('abc/exp')  
path.rename('abc/xxx')
# 将 abc 文件夹里面的 exp 重命名为 xxx
```
