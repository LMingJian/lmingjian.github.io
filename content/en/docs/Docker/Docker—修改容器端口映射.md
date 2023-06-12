---
title: 修改容器端口的映射
date: 2021-05-31
author: LM
---

## 1.找到 docker 的配置文件

使用 `docker ps -a` 命令找到要修改容器的 CONTAINER ID。

![](/images/drawingbed/img/202205050954001.png)

运行 `docker inspect 【CONTAINER ID】 | grep Id` 命令，根据容器 ID 找到文件 ID。

![](/images/drawingbed/img/202205050954517.png)

执行 `cd /var/lib/docker/containers` 命令进入 docker 容器文件夹，找到与文件 ID 相同的目录。

![](/images/drawingbed/img/202205050954626.png)

## 2.修改 docker 的配置文件

停止 docker 引擎服务，systemctl stop docker 或者 service docker stop。

进入对应文件 ID 所在目录，修改 hostconfig.json 文件。

![](/images/drawingbed/img/202205050954685.png)

把 8080 映照到 80，重启服务 systemctl start docker 。
