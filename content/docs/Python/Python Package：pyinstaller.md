---
title: Python Package：pyinstaller
date: 2024-12-25T16:11:37+08:00
author: LiangMingJian
---

# 概述

> 第三方库

PyInstaller 是 Python 中一个著名的打包库，它可以将代码及其所有依赖项都捆绑到一个包中。用户可以直接运行打包的应用程序，而无需安装 Python 解释器或任何模块。

特别注意，**PyInstaller 不是交叉编译工具**，它只能在 Windows 上打包 Windows 应用程序， 在 Linux 上打包 Linux 应用程序。

```python
pip install pyinstaller
```

# 基本使用

```python
pyinstaller [options] script.py

# 最通常使用的语句
pyinstaller -F -c script.py
```

打包后文件会出现在当前文件夹中新建的 dist 文件夹中。用户可以通过传入不同的 option，使用不同的打包功能。

# 打包为单个可执行文件

pyinstaller 默认会将代码打包为一个带运行库文件夹的应用程序，在某些电脑上可能会报毒。此时通过 `-F` 参数打包为单个可执行文件就很有必要。

```python
pyinstaller -F script.py
```

# 指定应用程序名

```python
pyinstaller -n myexe script.py
```

# 指定工作目录

在多层嵌套文件夹中打包 Python 程序时，有可能会因为工作路径不同导致部分导入模块查找失败，此时就应该使用 `-p` 参数指定工作目录。

```python
pyinstaller -p tools tools/script.py
```

# 显示控制台

**Windows 和 macOS 特定选项**

当脚本中存在 `print, input` 等输入输出代码时，必须使用 `-c` 参数携带控制台窗口，否则程序无法运行。

如果无需控制台显示，则可以携带参数 `-w` 关闭控制台。

```python
# 有控制台
pyinstaller -c script.py

# 没有控制台
pyinstaller -w script.py
```

# 添加 icon 图标

**Windows 和 macOS 特定选项**

```python
pyinstaller -i FILE.ico script.py
```

————————————

> [ PyInstaller Manual @PyInstaller 用户手册 ](https://pyinstaller.org/en/stable/index.html)
