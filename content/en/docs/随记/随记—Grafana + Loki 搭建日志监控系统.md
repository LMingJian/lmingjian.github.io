---
title: Grafana + Loki 搭建日志监控系统
date: 2022-06-22
author: LM
---

## 1.监控系统组成

通过搭建 Grafana，Loki 以及 Promtail 来建立日志监控系统。

Grafana 是由 Grafana Labs 公司开源的一个监控仪表系统。它可以帮助用户简化监控的复杂度，用户只需提供数据，便可以生成各种可视化仪表。同时还支持报警功能，可以在系统出现问题时通知用户。

而 Loki 是 Grafana Lab 公司开源的一个水平可扩展、高可用性、多租户的日志聚合系统。提供对日志的收集，建立标签索引的功能，实现对日志的监控。

需要注意的是，Loki 不主动的监控日志，它仅做收集功能。为了能够监控日志，需要在被监控机上使用 Promtail。Promtail 周期性的向 Loki 暴露的 HTTP 服务地址推送监控样本数据。其工作原理如下。

![](/images/drawingbed/img/202206221443693.png)

## 2.Grafana，Loki 安装

Grafana，Loki 使用 docker 安装，以避免环境配置的麻烦。

注意：需要映射 loki-config.yaml，/var/lib/grafana 目录到宿主机，以便于后续配置（ 映射目录需要赋予权限，不然容器无法读取写入 ）

```bash
wget https://raw.githubusercontent.com/grafana/loki/v2.5.0/cmd/loki/loki-local-config.yaml -O loki-config.yaml
docker run --name loki -d -v $(pwd):/mnt/config -p 3100:3100 grafana/loki:2.5.0 -config.file=/mnt/config/loki-config.yaml
mkdir /root/grafana
chmod 777 /root/grafana
docker run -d -p 3000:3000 --net=grafana --name=grafana --privileged=true -v /root/grafana:/var/lib/grafana grafana/grafana-oss
```

容器正常启动后，访问 IP:9090，IP:3100/metrics，IP/ready，Grafana 默认账号密码 admin-admin，

## 3.Promtail 安装

Promtail 的安装可以前往：[https://github.com/grafana/loki/releases](https://github.com/grafana/loki/releases)，下载对应的二进制文件，解压并执行。注意：需要将配置文件下载至对应文件夹，并指定需要监控的日志文件地址。

```bash
wget https://raw.githubusercontent.com/grafana/loki/v2.5.0/clients/cmd/promtail/promtail-docker-config.yaml -O promtail-config.yaml
./promtail-linux-amd64 -config.file=promtail-config.yaml
```

promtail-config.yaml

```yaml
server:
  http_listen_port: 0
  grpc_listen_port: 0

# 日志读取位置记录
positions:
  filename: /tmp/positions.yaml

# 配置你的 loki 地址和端口
clients:
  - url: http://localhost:3100/loki/api/v1/push

scrape_configs:
  - job_name: wms_main
    static_configs:
      - targets:
          - localhost
        labels:
          job: wms_main
          app: wms
          name: jtwms
          __path__: E:\IDEA_HOME\guohe\wms-main-api\logs\*.log
    pipeline_stages:
      - match:
          selector: '{job="wms_main"}'
          stages:
            - regex:
                expression: '^(?s)(?P<timestamp>\S+?) (?P<level>\S+?) (?P<content>.*)$'
            # 合并日志行
            - multiline:
                firstline: '^\d{4}-\d{2}-\d{2}T\d{1,2}:\d{2}:\d{2}\.\d{3}'
                max_wait_time: 3s
            - labels:
                level:
                content:
            # 格式化时间
            - timestamp:
                format: RFC3339Nano
                source: timestamp
  - job_name: wms_tools
    static_configs:
      - targets:
          - localhost
        labels:
          job: wms_tools
          __path__: E:\IDEA_HOME\guohe\wms-tools-api\logs\*.log
    pipeline_stages:
      - match:
          selector: '{job="wms_tools"}'
          stages:
            - regex:
                expression: '^(?s)(?P<timestamp>\S+?) (?P<level>\S+?) (?P<content>.*)$'
            - multiline:
                firstline: '^\d{4}-\d{2}-\d{2}T\d{1,2}:\d{2}:\d{2}\.\d{3}'
                max_wait_time: 3s
            - labels:
                level:
            - timestamp:
                format: RFC3339Nano
                source: timestamp
```

## 4.Grafana 配置

在 Grafana 中选择 Loki 数据源，进行 Explore 即可对日志进行查找。

{{< details "参考文件" >}} 
1：[ PLG日志平台搭建: Promtail + Loki + Grafana 全步骤_@dxcccccc ](https://blog.csdn.net/fwzzzzz/article/details/119003585)
{{< /details >}}