---
title: 通过固定 Host 解决 Docker 网络内部 DNS 异常的问题
date: 2026-04-20T10:21:49+08:00
author: LiangMingJian
---

# 问题

在工作中，遇见 Linux 服务器 Docker 内部 DNS 网络异常导致容器间无法通过各自容器名互相访问的问题。该问题在经历重启 Docker 服务，重启服务器后都无法解决，因此只能通过固定容器 IP，然后配置容器 Host 来保证服务的正常运行。

# 解决

通过在 `docker-compose.yml` 文件中添加 `networks` 配置项固定 Docker 网络，然后在容器中通过 `ipv4_address` 字段固定 IP 地址，最后通过 `extra_hosts` 来写入所有容器和 IP 的对应关系。

特别的，有多少服务，`extra_hosts` 就得写多少对应关系。

> 这里其实推荐让 AI 帮忙写一下，让 AI 列出所有容器和 IP 的 Host。当然，上述的 `extra_hosts` 也适用于配置容器内部访问外网的 Host。

然后，在 `yml` 文件中写入 `extra_hosts` 的时候，推荐使用锚点机制 `&data, *data` 来对数据进行引用，便于后续维护。

最终的 `docker-compose.yml` 文件如下所示：

```yaml
version: '3.1'

services:
  database:
    image: x.com/database:latest
    container_name: database
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=123456
      - MYSQL_DATABASE=platform
      - TZ=Asia/Shanghai
    ports:
      - "3306:3306"
    privileged: true
    tty: true
    volumes:
      - /home/database:/var/lib/mysql
    networks:
      my_network:
        ipv4_address: 10.11.0.10
    extra_hosts: &extra_hosts_list
      - "database:10.11.0.10"
      - "mongodb:10.11.0.11"
  mongodb:
    image: x.com/mongodb:latest
    container_name: mongodb
    restart: always
    ports:
      - "27017:27017"
    privileged: true
    volumes:
      - /home/mongodb:/data/db
    networks:
      my_network:
        ipv4_address: 10.11.0.11
    extra_hosts: 
      *extra_hosts_in

networks:
  my_network:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 10.11.0.0/16
```
