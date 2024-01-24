---
title: Zabbix Server 的部署
date: 2022-01-20
author: LM
weight: 3003
---

## 1.Zabbix Server

Zabbix Server 是 Zabbix 的一个重要组成组件，其是回收和统计可用性，完整性信息的中心组件。

Zabbix Server 包含存储所有配置、统计和操作数据的中央存储库，以及支持从任何地方和任何平台轻松访问的 Web 界面。

## 2.Docker 部署

Zabbix Server 推荐使用 Docker 部署，以避免环境配置的困难，对应的部署脚本如下：

```bash
docker network create --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 zabbix-net

docker run --name zabbix-mysql -t \
      -e MYSQL_DATABASE="zabbix" \
      -e MYSQL_USER="zabbix" \
      -e MYSQL_PASSWORD="123456" \
      -e MYSQL_ROOT_PASSWORD="123456" \
      --network=zabbix-net \
      --restart unless-stopped \
      -d mysql:8.0 \
      --character-set-server=utf8 --collation-server=utf8_bin \
      --default-authentication-plugin=mysql_native_password

docker run --name zabbix-gateway -t \
      --network=zabbix-net \
      --restart unless-stopped \
      -d zabbix/zabbix-java-gateway:alpine-5.4-latest

docker run --name zabbix-server -t \
      -e DB_SERVER_HOST="zabbix-mysql" \
      -e MYSQL_DATABASE="zabbix" \
      -e MYSQL_USER="zabbix" \
      -e MYSQL_PASSWORD="123456" \
      -e MYSQL_ROOT_PASSWORD="123456" \
      -e ZBX_JAVAGATEWAY="zabbix-gateway" \
      --network=zabbix-net \
      -p 10000:10051 \
      --restart unless-stopped \
      -d zabbix/zabbix-server-mysql:alpine-5.4-latest

docker run --name zabbix-web -t \
      -e ZBX_SERVER_HOST="zabbix-server" \
      -e DB_SERVER_HOST="zabbix-mysql" \
      -e MYSQL_DATABASE="zabbix" \
      -e MYSQL_USER="zabbix" \
      -e MYSQL_PASSWORD="123456" \
      -e MYSQL_ROOT_PASSWORD="123456" \
      --network=zabbix-net \
      -p 8000:8080 \
      --restart unless-stopped \
      -d zabbix/zabbix-web-nginx-mysql:alpine-5.4-latest

docker inspect -f='{{.Name}} {{.NetworkSettings.IPAddress}} {{.HostConfig.PortBindings}}' $(docker ps -aq)

```
