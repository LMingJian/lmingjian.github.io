---
title: 如何在 Linux 中检查端口占用情况
date: 2024-12-25T13:55:46+08:00
author: LiangMingJian
---

# 使用 lsof 检查

`lsof` 是一个列出当前系统打开文件的工具，可以使用它来查看端口占用的情况。

```bash
lsof -i:端口号
```

# 使用 netstat 检查

`netstat -tunlp` 可以用于展示 tcp，udp 端口和进程的相关情况。

```bash
netstat -tunlp | grep 端口号
```
