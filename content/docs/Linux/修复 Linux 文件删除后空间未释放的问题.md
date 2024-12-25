---
title: 修复 Linux 文件删除后空间未释放的问题
date: 2024-12-25T13:56:26+08:00
author: LiangMingJian
---

# BUG 描述

在 Linux 系统中，通过 `rm` 删除某些文件后，`df -hl`命令显示空间仍未被释放。

# Resolution

当一个文件删除后，文件系统目录中已经不存在，du 命令将不再统计。此时若还有进程持有该文件的句柄，那么该文件就没有真正从磁盘中删除，仍然占用内存空间。df 命令仍然会统计这个文件。

此时，需要使用`lsof -n | grep deleted`命令查看处于 deleted 状态的文件，通过`kill`掉对应进程来释放内容。

{{< details "参考文件" >}} 
1：[ 文件删除后，空间仍占用 @CSDN ](https://blog.csdn.net/weixin_44052055/article/details/105461148)
{{< /details >}}
