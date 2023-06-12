---
title: docker 安装脚本
date: 2023-05-09
author: LM
---

## 1.安装脚本

创建文件`docker_install.sh`，然后写入如下内容。

```bash
sudo yum remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
                docker-logrotate \
                docker-engine

sleep 2
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sleep 2
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sleep 2
sudo systemctl start docker

sleep 2
sudo systemctl enable docker

sleep 2
docker version

```

