---
title: 如何管理 Docker 的网络
date: 2024-12-25T11:13:21+08:00
author: LiangMingJian
---

# 网络管理

通过`network`对 docker 内部的网络环境进行管理。

```bash
docker network ls
docker network inspect python3.9 # 查看容器网络配置

docker inspect -f {{".NetworkSettings.IPAddress"}} python3.9 # 查看容器IP
docker inspect -f='{{.Name}} {{.NetworkSettings.IPAddress}} {{.HostConfig.PortBindings}}' $(docker ps -aq) # 列出所有容器对应的名称，端口，及IP
```
