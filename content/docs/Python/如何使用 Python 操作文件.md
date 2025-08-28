---
title: 如何使用 Python 操作文件
date: 2024-12-25T16:10:28+08:00
author: LiangMingJian
---

# 前言

在 Python 中，通常都是使用内置函数 `open()` 来读写一个文件。

# open 的结构

```python
open(file, mode='r', buffering=-1, encoding=None, 
errors=None, newline=None, closefd=True, opener=None)
```

上述代码所展示的是内置函数 `open()` 的定义结构。针对 `open()` 函数，该函数支持 8 个参数的输入，各个参数所支持的内容如下：

## file 参数

**必填参数。**

file 是一个 path-like object，表示的是将要打开的文件路径，它可以是路径字符串，也可以是 pathlib.Path 对象，甚至可以是文件描述符。

> 在 Linux 系统中，文件描述符是对打开文件或 I/O 设备的引用，遵循“一切皆文件”的理念，几乎任何能够读写的东西都可以通过文件描述符来访问，外接的设备自然也不例外。

```python
# 字符串路径
with open('file.txt') as f: pass

# pathlib.Path 对象
from pathlib import Path
with open(Path('file.txt')) as f: pass

# 文件描述符
import os
# 使用 os.open() 打开一个"文件"，并返回文件描述符
fd = os.open('/dev/tty', os.O_RDONLY)
with open(fd, closefd=False) as f: pass
# 文件描述符是系统资源，必须手动进行关闭
os.close(fd)
```

## mode 参数

**可选参数。**

mode 指明文件打开的模式，文件是只读打开，还是写入打开，其支持的模式字符如下所示：

- `r`：只读，默认的模式字符。
- `w`：只写，如果文件存在，则写入前会清空文件。
- `a`：只写，如果文件存在，则写入会在文件末尾追加。
- `x`：可选模式，独占创建，生成/读取文件时，如果文件已存在，则失败。
- `b`：可选模式，以二进制模式读取。
- `t`：可选模式，以文本模式读取，默认的选择，不需要显式指定。
- `+`：可选模式，打开并更新文件，同时支持读取和写入。

在上面的模式字符中，可选模式能与 `r, w, a` 合并传入，带来不同的功能。

以 `w` 为例：

- `w`：只写，如果文件存在则打开并清空。
- `wx`：只写，如果文件存在则报错。
- `w+`：同时支持写入和读取，可以进行读写交互。
- `wb`：以二进制的模式写入数据。
- `w+b`：同时支持以二进制的模式写入和读取数据。

## buffering 参数

**可选参数。**

buffering 指明程序在读取时使用的缓冲策略，其支持的模式如下：

- `-1`：自动管理。
- `0`：禁用缓冲，注意，这个只在二进制模式下生效。
- 大于`1`的整数值：程序会使用这个大小的缓冲区临时存放文件。

## encoding 参数

**可选参数。**

encoding 指明在文本模式下文件的编码格式名称，默认的编码格式依赖系统平台的设置。

常见的编码设置如下：

- `encoding='utf-8'`：Unicode 编码。
- `encoding='gbk'`：中文编码。

## errors 参数

**可选参数。**

errors 指明在文本模式下如何处理编码和解码错误，这个在读取未知编码的文件时十分有效，可以保证程序在遇到错误编码的时候继续读取，而不会抛出错误。支持的处理方案有：

- `errors='strict'`：默认值，出现错误时抛出异常。
- `errors='ignore'`：出现错误时忽略异常，直接跳过，不返回这个字符。
- `errors='replace'`：出现错误时将错误的字符替换成问号 ？。
- `errors='xmlcharrefreplace'`：只在写入的时候支持，将错误的字符替换为对应的 XML 字符引用，遵循格式：`&#nnn;`。
- `errors='surrogateescape'`：在读取和写入同时使用时，会将错误的字符表示为 U+DC80 至 U+DCFF 范围内的下方替代码位，在读取时替换，在写入时转换会原本的字节。
- `errors='backslashreplace'` ：使用 Python 的反向转义序列替换错误的字符
- `errors='namereplace'` ：只在写入的时候支持，将错误的字符按 `\N{...}` 格式自定义的反向转义序列进行替换。

> Python 的反向转义序列是指使用诸如 \\u0050，\\u0059，\\u0054 这样格式的字符来直接展示编码内容的序列。（Unicode 编码中，\u0050=P，\uu0059=Y，\u0054=T）

## newline 参数

**可选参数。**

newline 可以指明如何解析文件的换行符，它的值可以为`None`, `''`, `'\n'`, `'\r'` 和 `'\r\n'`。根据不同的值，会在读取或写入时将给定的值识别为换行符。

其中 `None` 是默认通用模式，自动识别以 `'\n'`，`'\r'` 或 `'\r\n'` 结尾的行，或在写入时自动写入对应的换行符。比如 Windows 写入的是 `\n` 那实际写入的会是 `\r\n`，Linux 写入的是 `\n` 那实际写入的还是 `\n`。

当使用 `''` 时，代码会禁止换行符转换，原有的换行符会直接读取或写入。

## closefd 参数

**可选参数。**

closefd 用来控制文件描述符的关闭行为，默认是 True，当文件对象被关闭时，‌同时关闭底层文件描述符‌。

需要注意的是，如果 file 参数是路径，那么 closefd 的值必须是 True，否则程序会报错。当 file 的值是文件描述符时，closefd 可以是 False，在文件对象关闭后，不关闭文件描述符，可以让用户继续操作。

## opener 参数

**可选参数。**

opener 可以传入函数名，让用户使用自定义的文件打开函数来替代默认的打开行为，比如实现加密文件处理，特殊的文件权限管理等。比如下面代码：

```python
import os

def custom_opener(file, flags):
    print('我被解密了')
    fd = os.open(file, flags)
    return fd

with open('encrypted.txt', 'r', opener=custom_opener) as f:
    content = f.read()
    print(content)
```

# open 的使用

对应 open 的使用，通常我们建议使用 with 语句进行：

```python
with open('file.txt') as f:
    content = f.read()
    print(content)
```

通过 with 语句，open 函数打开的文件资源能被自动管理。避免因为报错，遗漏等原因，在打开文件后忘记关闭，导致系统资源一直被占用，进而导致程序崩溃。

当然，如果文件需要长期保存打开状态（这种情况很少见），那么还是可以使用传统方式来调用文件，比如：

```python
f = open('file.txt', 'r')
try:
    data = f.read()
finally:
    f.close()
```

在使用传统方式调用文件后，请务必保证文件被正确关闭，即使出现异常。

# 如何读取文件

在 Python 中，文件对象支持 3 种读取方式，分别是：`read()、readline()、readlines()`。

## read 方法

`read()` 方法会读取整个文件，并将文件内容放到一个字符串中。

特别的，读取的文件内容不能过大，否则会导致内存溢出。可以通过传入参数 `read(size)` 来指定每次读取的数据，单位字节。

## readline 方法

`readline()` 方法只会读取文件中的第一行，以对应系统的换行符进行区分（Windows 是 `\r\n`，Linux 是 `\n`），返回一个字符串。

## readlines 方法

`readlines()` 方法会读取整个文件，但是这个方法会将文件内容按换行符进行分割，将每一行的内容存为一个字符串，然后将所有行的内容作为列表进行返回。

# 如何写入文件

在 Python 中，文件对象支持 2 种写入方式，分别是：`write()、writelines()`。

## write 方法

和 `read()` 对应，该方法直接将字符串写入文件，传入参数是字符串。

## writelines 方法

和 `readlines()` 对应，该方法将一个字符串列表的数据写入文件，传入参数是字符串列表。

特别的，`writelines()` 不会自动添加换行符，它只是将列表中的每一个字符串按顺序原样写入，比如：

```python
str1 = ['I', 'Love', 'Python']
# 这个字符串 writelines() 写入时，不会自动分行
# 文件结果是：ILovePython

str2 = ['I\n', 'Love\n', 'Python\n']
# 这个字符串 writelines() 写入时，因为转义字符 \n，所以会自动分行
# 文件结果是：
# I
# Love
# Python
```