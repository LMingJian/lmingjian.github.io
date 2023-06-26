---
title: Zabbix 的 Docker 部署
date: 2022-01-20
author: LM
---

## 1.Zabbix server Docker 部署脚本

Zabbix 服务器端推荐使用 Docker 部署，以避免环境配置的困难。相应的部署脚本如下。

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

