---
title: 如何使用 Python 操作文件夹
date: 2024-12-25T16:10:30+08:00
author: LiangMingJian
---

# 前言

文件夹，又可以称为目录。在编程的时候，我们可能会因为配置文件读取、结果写入、数据导入等理由，需要对文件夹进行操作。这些操作可以包括：获取当前工作路径、创建文件夹、删除文件夹、查看文件夹里面文件列表等。此时，为实现上面的需求，我们可以使用 os 这一模块，来对文件夹进行操作。

> os 模块包含多种操作系统接口，可以帮助我们完成操作文件，文件夹，命令行等相关的工作。  
> os 模块的文档可以查阅： [os 模块文档、Ver 3.10](https://docs.python.org/zh-cn/3.10/library/os.html)

# 检查文件夹权限

在正式开始操作前，建议先对文件夹进行权限检查，避免因为权限不足导致的操作失败。

在 Python 中，使用 `os.access()` 来对文件夹权限进行验证。

```python
import os

if os.access("exfile", os.F_OK):
    print('OK')
```

上面代码中的 `os.F_OK` 是 `access` 方法的 mode 参数，用来测试文件夹的存在性，其余的 mode 包括：

- `os.F_OK`：存在性
- `os.R_OK`：可读性
- `os.W_OK`：可写性
- `os.X_OK`：可执行性

其中，对于可读、可写、可执行这些文件夹权限，可以使用位运算来批量检查，比如下面代码，通过位运算检查一个文件夹是否同时可读，可写，以及可执行：

```python
import os

# 检查是否可读、可写、可执行
if os.access("exfile", os.R_OK | os.W_OK | os.X_OK): 
    print('OK')
```

**特别的，上面方法对文件也有效，同时在 Windows 中，可读、可写、可执行这些判断只对文件生效，文件夹判断后无论是否只读，都返回真。**

# 获取当前的工作文件夹

在 Python 中，可以使用 `os.getcwd()` 来获取当前项目的工作文件夹。

> 获取工作文件夹的路径能让我们在使用相对路径调用文件时的思路更为清晰。

```python
import os

print(os.getcwd())
# 输出：F:\Mypython\PythonProject
```

# 改变工作文件夹

存在如下的文件夹结构：

```python
- exfile
  - test.txt
- example.py
```

此时，我们想要在 `example.py` 文件里编写读取文件夹 `exfile` 中 `test.py` 文件的代码，但我们是这样编写代码的：

```python
with open('test.txt', 'r') as f:
    pass
```

不难看出，这段代码必然报错，因为这是使用相对路径编写的，当前工作文件夹下没有 `test.py` 文件。

```python
FileNotFoundError: [Errno 2] No such file or directory: test.txt
```

要解决这个问题，正常我们使用 `exfile/test.txt` 来指定文件夹里面的文件，但今天，我们可以通过切换工作文件夹的方式来实现这个工作。

```python
import os

os.chdir('exfile')

with open('test.txt', 'r') as f:
    pass
```

通过方法 `os.chdir()` ，我们可以切换当前的工作文件夹。

# 修改文件夹权限

在 Python 中，我们可以使用 `os.chmod()` 来修改文件夹的权限。

```python
import os
import stat

os.chmod('usfile', stat.S_IREAD)
```

在上面代码中，`stat.S_IREAD` 是修改后的权限 mode，这里的 mode 同样支持使用位运算来批量检查，比如：`stat.S_IREAD | stat.S_IWRITE` 是可读可写权限。

常见的 mode 支持：

- `stat.S_ISVTX`：此文件夹中的文件只能由文件所有者、文件夹所有者或特权进程来重命名或删除。
- `stat.S_IROTH`：其他人具有读取权限。
- `stat.S_IWOTH`：其他人具有写入权限。
- `stat.S_IXOTH`：其他人具有执行权限。
- `stat.S_IRUSR`：所有者具有读取权限。
- `stat.S_IWUSR`：所有者具有写入权限。
- `stat.S_IXUSR`：所有者具有执行权限。
- `stat.S_IREAD`：可读。
- `stat.S_IWRITE`：可写。
- `stat.S_IEXEC`：可执行。

> 详细支持的列表，请查阅文档：[os.chmod](https://docs.python.org/zh-cn/3.10/library/os.html#os.chmod)

**特别的，在 Windows，只能用它设置文件的只读标志，即 `stat.S_IREAD` 和 `stat.S_IWRITE`，并无实际用途，管理员照样可写可读。**

## 列出文件夹下所有内容

存在如下的文件夹结构：

```python
- exfile
  - 666
    - 8888
    - b.py
    - test.txt
  - usfile
  - example.py
```

在 `example.py` 里，我们使用 `os.listdir('exfile')` 来打印 `exfile` 里面所有的内容。

```python
import os

print(os.listdir('exfile'))
# ['666', 'b.py', 'test.txt']
```

注意，该方法返回的是列表。

同时，该方法不会返回嵌套文件夹里面的内容，仅返回嵌套文件夹的名字。

另外，在 Windows 中，如果文件夹存在隐藏文件，这些隐藏文件也会被打印。但在 Linux 中，特殊内容 `'.'` 和 `'..'` 即使存在，也不会被打印。

**特别的，如果想对文件夹里面的内容进行遍历处理，又关注性能，那么可以使用 `os.scandir()` 来生成一个迭代器，返回一个 `os.DirEntry` 对象进行处理。比如：**

```python
with os.scandir(path) as it:
    for entry in it:
        if entry.is_file():
            print(entry.name)
```

> `os.DirEntry` 支持的属性和方法请查阅文档： [os.DirEntry](https://docs.python.org/zh-cn/3.10/library/os.html#os.DirEntry)

# 创建文件夹

在 Python 中，使用 `os.mkdir` 创建文件夹。

```python
import os

os.mkdir('bsfile')
```

注意，如果文件夹已存在，则程序会报错 `FileExistsError`。

```python
FileExistsError: [WinError 183] 当文件已存在时，无法创建该文件。:'bsfile'
```

另外，该方法一次只能建立一个文件夹，且该文件夹不能是嵌套文件夹。

```python
import os

os.mkdir('asfile/zsfile')
```

如果建立嵌套文件夹，则代码会报错：

```python
FileNotFoundError: [WinError 3] 系统找不到指定的路径。:'asfile/zsfile
```

**特别的，如果想一次性创建嵌套文件夹，请使用 `os.makedirs()`，比如：**

```python
import os

os.makedirs('asfile/zsfile')
```

# 删除文件夹

在 Python 中，删除文件夹只能删除空文件夹，如果文件夹里面存在内容，则无法进行删除。

使用 `os.rmdir` 来移除一个文件夹。

```python
import os

os.rmdir('0123')
```

对于嵌套的文件夹的删除，可以使用 `os.removedirs` 来一级一级删除。

```python
import os

os.removedirs('asfile/zsfile')
```

**特别的，如果想删除文件，可以使用 `os.remove()`，注意，这个方法只能用来删除文件，如果传来的地址是文件夹，则会抛出 OSError。**

**特别的，如果想删除一个带有内容的文件夹，可以使用 `shutil.rmtree()`，如下：**

```python
import shutil

shutil.rmtree('exfile')
```

# 重命名文件夹

在 Python 中，使用 `os.rename()` 来实现文件夹的重命名，比如下面代码，将文件夹 0123 重命名为 3210：

```python
import os

os.rename('0123', '3210')
```

# 遍历文件夹

在 Python 中，使用 `os.walk()` 不仅可以遍历当前文件夹下所有内容，还可以遍历子文件夹下所有内容，直到所有子文件夹都被遍历。

`os.walk()` 返回的是一个生成器对象，通过迭代这个对象可以获取一个包含（当前文件夹名，当前文件夹下所有子文件夹名列表，当前文件夹下所有文件名列表）的三元组。

有文件结构如下：

```python
- 3210
  - 3
    - A.py
    - 4
    - 5
    - 1.py
    - 2.py
  - example.py
```

有代码如下：

```python
import os

for root, dirs, files in os.walk('3210'):
    print(f"目录: {root}")
    print(f"子目录: {dirs}")
    print(f"文件: {files}\n")
```

其执行结果：

```python
目录:3210
子目录:['3', '4', '5']
文件:['1.Py', '2.Py']

目录:3210\3
子目录:[]
文件:['A.Py']

目录:3210\4
子目录:[]
文件:[]

目录:3210\5
子目录:[]
文件:[]
```