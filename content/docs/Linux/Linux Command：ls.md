---
title: Linux Command：ls
date: 2024-12-25T13:58:05+08:00
author: LiangMingJian
---

# 概述

`ls`（ list directory contents ）命令用于显示指定工作目录下之内容（列出目前工作目录所含的文件及子目录)。

```
ls [-alrtAFR] [name...]
```

# 支持的参数

```
ls -l                    # 以长格式显示当前目录中的文件和目录  
ls -a                    # 显示当前目录中的所有文件和目录，包括隐藏文件  
ls -lh                   # 以人类可读的方式显示当前目录中的文件和目录大小  
ls -t                    # 按照修改时间排序显示当前目录中的文件和目录  
ls -R                    # 递归显示当前目录中的所有文件和子目录  
ls -l /etc/passwd        # 显示/etc/passwd文件的详细信息
```

# Ex.Linux Command：ll

特别的，部分系统可使用`ll`命令来列出文件的详细信息，等同于`ls -l`。
