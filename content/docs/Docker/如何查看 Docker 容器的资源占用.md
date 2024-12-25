---
title: 如何查看 Docker 容器的资源占用
date: 2024-12-25T11:13:06+08:00
author: LiangMingJian
---

# docker stats

`docker stats` 用来显示容器使用的系统资源，默认情况下，stats 命令会每隔 1 秒钟刷新一次输出内容。输出的内容包括：

- CONTAINER：以短格式显示容器的 ID。
- CPU %：CPU 的使用情况。
- MEM USAGE / LIMIT：当前使用的内存和最大可以使用的内存。
- MEM %：以百分比的形式显示内存使用情况。
- NET I/O：网络 I/O 数据。
- BLOCK I/O：磁盘 I/O 数据。 
- PIDS：PID 号。

# docker stats --no-stream

通过 --no-stream 选项，可以使命令只输出一次的资源占用情况，而不进行刷新。

# docker stats  容器 ID / Name

显式指定容器 ID 或名称，进查看某个容器的资源占用。

{{< details "参考文件" >}} 
1：[ 查看 Docker 容器使用资源情况 @CSDN ](https://blog.csdn.net/QMW19910301/article/details/88058769)
{{< /details >}}
