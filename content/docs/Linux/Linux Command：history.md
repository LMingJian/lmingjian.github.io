---
title: Linux Command：history
date: 2024-12-25T13:57:55+08:00
author: LiangMingJian
---

# 概述

history 命令用于显示历史记录和执行过的指令记录。

如想查询某个用户在系统上执行了什么命令，可以使用 root 用户登录系统，检查 Home 目录下的 .bash_history 文件，该文件记录了用户所使用的命令和历史信息。

```
history (选项) (参数)
```

# 支持的参数

```
n:  打印最近的n条历史命令；
-N: 显示历史记录中最近的 N 个记录；
-c：清空当前历史命令；
-a：将历史命令缓冲区中命令写入历史命令文件中；
-r：将历史命令文件中的命令读入当前历史命令缓冲区；
-w：将当前历史命令缓冲区命令写入历史命令文件中；
-d <offset>：删除历史记录中第 offset 个命令；
-n <filename>：读取指定文件；
```
