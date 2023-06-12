---
title: docker 命令
date: 2021-08-16
author: LM
---

## 1.运行 docker

`docker`以服务形式安装在系统中，需要使用`systemctl`命令控制开启或关闭。

```bash
systemctl start docker # 运行docker
systemctl status docker # 查看docker状态
systemctl enable docker  # 自启动docker
```

## 2.版本

通过`docker version`命令来查看 docker 版本。

## 3.容器管理

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

## 4.镜像管理

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

## 5.网络管理

通过`network`对 docker 内部的网络环境进行管理。

```bash
docker network ls
docker network inspect python3.9 # 查看容器网络配置

docker inspect -f {{".NetworkSettings.IPAddress"}} python3.9 # 查看容器IP
docker inspect -f='{{.Name}} {{.NetworkSettings.IPAddress}} {{.HostConfig.PortBindings}}' $(docker ps -aq) # 列出所有容器对应的名称，端口，及IP
```

## 6.文件复制

通过`cp`命令进行文件的复制。

```bash
docker cp testtomcat：/usr/local/tomcat/webapps/test/js/test.js /opt  # 从容器里面拷文件到宿主机
docker cp /opt/test.js testtomcat：/usr/local/tomcat/webapps/test/js  # 从宿主机拷文件到容器里面
```



