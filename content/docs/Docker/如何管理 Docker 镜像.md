---
title: 如何管理 Docker 镜像
date: 2024-12-25T11:13:25+08:00
author: LiangMingJian
---

# 镜像管理

执行`images`命令可以展示当前所有的镜像，`rmi`命令删除镜像。

```bash
docker images

docker rmi image
docker rmi $(docker images -q)  # 删除所有镜像
```

通过`export`命令可以将一个镜像导出成压缩文件。

```bash
docker export a9ad7f0cb1ad > $(pwd)/itestserver.tar
```

通过`search`命令可以对镜像搜索，然后通过`pull`命令进行拉取下载。

```bash
docker search ubuntu
docker pull ubuntu
```

通过`tag`命令可以对镜像进行重命令。

```bash
docker tag [镜像id] [新镜像名称]:[新镜像标签]
docker tag 93109ce1d590 test/python-env:2.7.13
```

在错误的为一个镜像添加多个标签时，可以通过 `rm` 命令删除多余镜像的标签

```bash
docker image rm nginx:1.16.0
```
