---
title: 如何管理 Docker 的网络
date: 2024-12-25T11:13:21+08:00
author: LiangMingJian
---

# 查看网络信息

通过`network`对 docker 内部的网络环境进行管理。

```bash
docker network ls
docker network inspect my_network # 查看网卡的配置
docker inspect gateway  # 查看容器的配置
```

# 清除不要的网络

```
docker network prune
```
