---
title: 容器的后台运行
date: 2021-08-17
author: LM
---

## 1.使用参数 -d

使用`-d`参数，如`docker run -d python bash`，实现容器的后台运行。

需要注意的是，docker 在后台运行时，必须有一个前台进程。如果这个进程运行的命令不是能一直挂起的命令（如 ping，sleep），那么在执行完命令后，进程就会退出，同时 docker 也就会停止。

## 2.解决方法

### 1.执行挂起阻塞命令

```bash
docker run -d python sleep 99999999999999
```

### 2.使用交互界面后退出容器

```bash
docker run -it python /bin/bash
```

