---
title: 如何查看 Docker 容器的资源占用
date: 2024-12-25T11:13:06+08:00
author: LiangMingJian
---

# 查看容器的资源占用

在 docker 中，`docker stats` 用来显示容器使用的系统资源。

默认情况下，调用 stats 命令，系统会每隔 1 秒钟刷新一次输出内容。

输出的内容包括：

- CONTAINER：以短格式显示的容器 ID。
- CPU %：CPU 的使用情况。
- MEM USAGE / LIMIT：当前使用的内存和最大可以使用的内存。
- MEM %：以百分比的形式显示的内存使用情况。
- NET I/O：网络 I/O 数据。
- BLOCK I/O：磁盘 I/O 数据。 
- PIDS：PID 号。

![](_images/drawingbed/img/Pasted%20image%2020260526163659.png)

另外，在使用 `docker stats` 时，因为 `stats` 命令会每秒刷新一次，因此界面会不停闪烁，同时当容器过多时，你可能没办法查看顶部的数据（数据一直刷新，导致无法滚动）。

为了解决上面的这个问题，一般建议添加 `--no-stream`，让命令只输出单次的资源占用情况，不每秒进行刷新。

**执行命令**： `docker stats --no-stream`。

![](_images/drawingbed/img/Pasted%20image%2020260526164017.png)

最后，当需要显示指定容器的占用时，通过携带容器 ID 或容器名称进行查看。

**执行命令**： `docker stats <container id/name>`。

# 查看容器内进程的资源占用

要查看 docker 容器内的资源占用，除了进入容器内执行 top 命令外，我们也可以直接在外面通过 `docker top <container id/name>` 来执行。

![](_images/drawingbed/img/Pasted%20image%2020260526165424.png)

当然，除了通过 top 命令查看资源占用外，因为 docker 调用的进程本质还是由 Linux 管理的，因此还可以通过 Linux 自带的命令 `ps aux` 查看系统所有进程的运行状态和资源占用，识别异常进程。

![](_images/drawingbed/img/Pasted%20image%2020260526165646.png)

同样的，通过 `docker exec <container id/name> ps aux` 可以在容器内执行命令，检查进程状态。

![](_images/drawingbed/img/Pasted%20image%2020260526165843.png)

————————

> [ 查看 Docker 容器使用资源情况 @CSDN ](https://blog.csdn.net/QMW19910301/article/details/88058769)
