---
title: Python 模块：pyinstaller
date: 2021-11-22
author: LM
---

## 1.简介

pyinstaller 是一个生成可执行文件的打包工具，其在使用时会根据平台不同打包不同的可执行文件，在 Windows 上打包 Windows exe 应用，在 Linux 上打包 Linux 的，不能交叉打包。

```
pip install pyinstaller
```

## 2.使用

```python
# pyinstaller -F(单个可执行文件) -n 程序名 -w(去掉控制台窗口) -c(携带控制台窗口) -i 图标.ico 脚本文件
pyinstaller -F test/Demo.py
pyinstaller -F -w test/Demo.py
pyinstaller -F -c test/Demo.py
# 指定工作台目录，避免出现无法找到运行库的问题
pyinstaller -p tools -F -c tools/live_compute.py
```

PS：当脚本中存在 `print，input` 代码时，必须使用 `-c` 参数携带控制台窗口，否则程序会无法运行。

{{< details "参考文件" >}} 
1：[ PyInstaller Manual @PyInstaller 4.6 documentation ](https://pyinstaller.readthedocs.io/en/stable/)
{{< /details >}}