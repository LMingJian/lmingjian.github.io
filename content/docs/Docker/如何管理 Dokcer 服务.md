---
title: 如何管理 Dokcer 服务
date: 2024-12-25T11:13:30+08:00
author: LiangMingJian
---

# 运行 docker

`docker`以服务形式安装在系统中，需要使用`systemctl`命令控制开启或关闭。

```bash
systemctl start docker # 运行 docker
systemctl status docker # 查看 docker 状态
systemctl enable docker  # 自启动 docker
```

# 查看版本

通过`docker version`命令来查看 docker 版本。
