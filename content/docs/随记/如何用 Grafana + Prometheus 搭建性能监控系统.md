---
title: 如何用 Grafana + Prometheus 搭建性能监控系统
date: 2024-12-25T10:32:08+08:00
author: LiangMingJian
---

# 监控系统组成

通过搭建 Grafana，Prometheus 以及 Exporter 来建立性能监控系统。

Grafana 是由 Grafana Labs 公司开源的一个监控仪表系统。它可以帮助用户简化监控的复杂度，用户只需提供数据，便可以生成各种可视化仪表。同时还支持报警功能，可以在系统出现问题时通知用户。

Prometheus 同样是 Grafana Labs 公司开源的一个时间序列数据库。Prometheus 主要用于对基础设施的监控，包括服务器、数据库、Web服务等。Prometheus 并不直接监控特定的目标的各项指标，比如 linux 系统的各项数据。它主要任务是数据的收集，存储并对外提供数据查询支持，监控数据的获取通过建立与数据源的联系来实现。

需要注意的是，Prometheus 不对数据进行采集，为了能够监控到某些东西，如主机的 CPU 使用率，需要在被监控机上使用 Exporter。Exporter 是一个相对开放的概念，并不专门指某一个程序。它可以是一个独立运行的程序，独立于监控目标以外 ( 如 Node Exporter，独立于操作系统，却能获取到系统各类指标 ) 。也可以是直接内置在监控目标中的代码 ( 如在项目代码层面接入普罗米修斯 API，实现指标上报 ) 。总结下来就是，只要能够向 Prometheus 提供标准格式的监控样本数据，那就是一个 Exporter，Prometheus 周期性的从 Exporter 暴露的 HTTP 服务地址中拉取监控样本数据。

Grafana，Prometheus，Exporter 间的工作关系如下图。

![](/_images/drawingbed/img/202206221124598.png)

# Grafana，Prometheus 安装

Grafana，Prometheus 使用 docker 安装，以避免环境配置的麻烦。注意：需要映射 /etc/prometheus，/var/lib/grafana 目录到宿主机，以便于后续配置（ 映射目录需要赋予权限，不然容器无法读取写入 ）

```bash
docker network create grafana
mkdir /root/prometheus
chmod 777 /root/prometheus
docker run -d -p 9090:9090 --net=grafana --name=prometheus --privileged=true -v /root/prometheus:/etc/prometheus prom/prometheus
mkdir /root/grafana
chmod 777 /root/grafana
docker run -d -p 3000:3000 --net=grafana --name=grafana --privileged=true -v /root/grafana:/var/lib/grafana grafana/grafana-oss
```

容器正常启动后，访问 IP:9090，IP:3000，Grafana 默认账号密码 admin-admin

# Exporter 安装

Exporter 的安装可以前往：[https://github.com/prometheus](https://github.com/prometheus) ，根据需求和被监控机的类型进行选择。以监控 Linux 主机的 Node_Exporter 举例，选择对应的版本下载并解压，进入文件夹后执行`./node_exporter` 即可开始监控，可以访问 IP:9100 查看服务运行状态

```bash
nohup ./node_exporter > ./node_exporter.log 2>&1 &
ps aux | grep "node_exporter"
kill PID
```

# Prometheus 添加被监控机

在目录 /etc/prometheus 中添加配置文件 prometheus.yml ，Prometheus 会根据文件内容对涉及主机进行监控。文件内容如下：

```yaml
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']
```

# Grafana 配置

## a.配置数据源

选择 Configuration 的 Data Sources，点击 Add data source，选择 Prometheus，在 URL 一栏填写 Prometheus 的 HTTP 访问地址，保存即可。

![](/_images/drawingbed/img/202206221157251.png)

## b.设置面板

Grafana 提供了官方的 Dashboard 市场：[https://grafana.com/grafana/dashboards](https://grafana.com/grafana/dashboards)，用户可以在里面选择需要的面板，然后在 Grafana 中点击 Import，输入编号添加，如面板 [ Linux Hosts Metrics | Base dashboard for Grafana | Grafana Labs ](https://grafana.com/grafana/dashboards/10180)，10180。

![](/_images/drawingbed/img/202206221342648.png)

## c.将面板设为首页

Grafana 允许将收藏的面板设置为首页，在 Preferences 中设置 Home Dashboard。

![](/_images/drawingbed/img/202206221349818.png)

{{< details "参考文件" >}} 
1：[ 一文搞懂Prometheus、Grafana  @yuann ](https://cloud.tencent.com/developer/article/1769920)
2：[ Prometheus + Grafana搭建可视化监控系统 @ZongweiBai ](https://www.cnblogs.com/zongwei/p/13937017.html)
{{< /details >}}
