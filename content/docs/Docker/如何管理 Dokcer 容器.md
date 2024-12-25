---
title: 如何管理 Dokcer 容器
date: 2024-12-25T11:13:35+08:00
author: LiangMingJian
---

# 容器管理

通过`ps`命令来查看容器，`rm`命令删除容器。

```bash
docker ps -a  # 查看容器，包括未运行
docker ps  # 查看容易，正在运行的

docker rm name # 删除
docker rm -f name # 强制删除
docker rm $(docker ps -qa)  # 删除所有容器
```

通过`run`命令来运行一个容器，也可以通过`exec`命令让一个容器执行任务。

```bash
docker run --name python3.9 --privileged=true -v /root/pythonScript:/pythonScript -it python /bin/bash
# name 名字，privileged 权限，-v 挂载目录，-it 以shell模式
# -p 宿主机:容器

docker exec -it id /bin/bash # 进入容器(以命令行方式执行 /bin/bash 命令)
>>> exit # 容器内退出
docker exec id commend  # 执行命令
```

支持如`start, stop, restart, rename`等命令。

```bash
docker start container
docker stop container
docker stop $(docker ps -q)  # 停用全部运行中的容器
docker restart container
docker rename gallant_swartz python3.9 # 重命名
```

# 容器提交

```bash
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
docker commit -a="mrhelloworld" -m="jdk11 and tomcat9" centos7 mycentos:7
```

- `-a`：提交的镜像作者；
- `-c`：使用 Dockerfile 指令来创建镜像；
- `-m`：提交时的说明文字；
- `-p`：在 commit 时，将容器暂停。
