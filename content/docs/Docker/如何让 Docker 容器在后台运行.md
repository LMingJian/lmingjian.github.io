---
title: 如何让 Docker 容器在后台运行
date: 2024-12-25T11:13:39+08:00
author: LiangMingJian
---

# 使用参数 -d

使用`-d`参数，如`docker run -d python bash`，实现容器的后台运行。

需要注意的是，docker 在后台运行时，必须有一个前台进程。如果这个进程运行的命令不是能一直挂起的命令（如 ping，sleep），那么在执行完命令后，进程就会退出，同时 docker 也就会停止。

可以使用以下的方法来阻塞进程：

- 执行阻塞命令：`docker run -d python sleep 99999999999999`
- 使用交互界面后退出容器：`docker run -it python /bin/bash`
