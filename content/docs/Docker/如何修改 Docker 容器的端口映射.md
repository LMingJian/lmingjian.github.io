---
title: 如何修改 Docker 容器的端口映射
date: 2024-12-25T11:13:49+08:00
author: LiangMingJian
---

# 修改配置文件

使用 `docker ps -a` 命令找到要修改容器的 CONTAINER ID。

![](/_images/drawingbed/img/202205050954001.png)

运行 `docker inspect 【CONTAINER ID】 | grep Id` 命令，根据容器 ID 找到文件 ID。

![](/_images/drawingbed/img/202205050954517.png)

执行 `cd /var/lib/docker/containers` 命令进入 docker 容器文件夹，找到与文件 ID 相同的目录。

![](/_images/drawingbed/img/202205050954626.png)

停止 docker 引擎服务，systemctl stop docker 或者 service docker stop。

进入对应文件 ID 所在目录，修改 hostconfig.json 文件。

![](/_images/drawingbed/img/202205050954685.png)

把 8080 映照到 80，重启服务 systemctl start docker 。
