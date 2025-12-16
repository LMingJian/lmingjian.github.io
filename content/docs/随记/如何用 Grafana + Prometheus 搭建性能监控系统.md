---
title: 如何用 Grafana + Prometheus 搭建性能监控系统
date: 2024-12-25T10:32:08+08:00
author: LiangMingJian
---

# 监控系统组成

通过搭建 Grafana，Prometheus 以及 Exporter 来建立性能监控系统。

**Grafana** 是由 Grafana Labs 公司开源的一个监控仪表系统。它可以帮助用户简化监控的复杂度，用户只需提供数据，便可以生成各种可视化仪表。同时还支持报警功能，可以在系统出现问题时通知用户。

**Prometheus** 同样是 Grafana Labs 公司开源的一个时间序列数据库。它并不直接监控特定目标的各项指标，比如 CPU，内存，存储数据，它主要任务是数据的收集和存储，通过统计分析对外提供数据查询支持。

**Exporter** 是客户机上用来获取数据的一个程序。因为 Prometheus 不对数据进行采集，因此，为了能够监控到系统的资源使用情况，就需要在客户机上安装 Exporter 来捕获数据。Prometheus 周期性的从 Exporter 暴露的 HTTP 服务地址中拉取监控样本数据。

> 需要注意 Exporter 是一组程序的概念，它并不只是某一个特定程序，不同的数据捕获需求需要使用不同的 Exporter 捕获程序。

Grafana，Prometheus，Exporter 间的工作关系如下图。

![](/_images/drawingbed/img/202206221124598.png)

Grafana，Prometheus 以及 Exporter 的对应的官方网站如下：

[ Grafana Download ](https://grafana.com/grafana/download)

[ Prometheus Download ](https://prometheus.io/download/)

[ Exporters ](https://prometheus.io/docs/instrumenting/exporters/)

# Grafana 安装

在这里，由于当前国内无法访问官方 docker 仓库，因此为了避免增加工作量，这里选择直接下载相应的二进制源代码文件手动安装运行。

首先，根据你的系统，点击 [下载页面](https://grafana.com/grafana/download) 上的**Linux**或**ARM**标签，下载目标的文件。

![](_images/drawingbed/img/Pasted%20image%2020251210171440.png)

比如，Linux AMD 64 位安装包：[ grafana_12.3.0_19497075765_linux_amd64.tar.gz ](https://dl.grafana.com/grafana/release/12.3.0/grafana_12.3.0_19497075765_linux_amd64.tar.gz)

然后在下载完成后，将下载的安装包上传至服务器，执行以下命令解压：

```bash
tar -zxvf grafana_12.3.0_19497075765_linux_amd64.tar.gz
```

解压完成后，按顺序执行以下指令，完成 Grafana 服务安装：

```bash
# 创建一个 Grafana 用户
useradd -r -s /bin/false grafana

# 将解压后的安装包移动到 /usr/local/grafana
mv ./grafana-12.3.0 /usr/local/grafana

# 将 /usr/local/grafana 这个文件夹的所有者改为 Grafana
chown -R grafana:users /usr/local/grafana

# 复制默认配置为新的配置 grafana.ini
cp /usr/local/grafana/conf/defaults.ini /usr/local/grafana/conf/grafana.ini

# 创建 Grafana 服务的 systemd 文件（文件内容见下文）
touch /etc/systemd/system/grafana-server.service
vi /etc/systemd/system/grafana-server.service

# 手动启动服务，等待服务完成安装配置（出现 Usage stats are ready to report）
/usr/local/grafana/bin/grafana server --homepath /usr/local/grafana

# 在完成配置后，结束服务器运行
Ctrl + C

# 再次更新 /usr/local/grafana 所有者为 Grafana
chown -R grafana:users /usr/local/grafana
```

systemd 文件 `grafana-server.service` 的内容

```ini
[Unit]
Description=Grafana Server
After=network.target

[Service]
Type=simple
User=grafana
Group=users
ExecStart=/usr/local/grafana/bin/grafana server --config=/usr/local/grafana/conf/grafana.ini --homepath=/usr/local/grafana
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

在上述操作完成后，Grafana 就作为一个服务安装到 Linux 系统上了，此时可以通过 `systemctl` 命令来控制服务的启动，停止。

```bash
# 启动
systemctl daemon-reload
systemctl start grafana-server

# 开机自启
systemctl enable grafana-server.service

# 运行状态
systemctl status grafana-server.service
```

Grafana 默认访问端口为 3000（请注意开放端口），默认的账号密码为 admin - admin。

```bash
firewall-cmd --zone=public --add-port=3000/tcp --permanent
firewall-cmd --reload
```

![](_images/drawingbed/img/Pasted%20image%2020251210175000.png)

# Prometheus 安装

在这里，同样因为无法使用 docker，所以 Prometheus 也要使用二进制源代码进行安装。

首先，根据你的系统，在 [下载页面](https://prometheus.io/download/) 上找到你所需要版本的二进制源代码文件。

![](_images/drawingbed/img/Pasted%20image%2020251211152844.png)

比如，Linux AMD 64 位安装包：[ prometheus-3.5.0.linux-amd64.tar.gz ](https://github.com/prometheus/prometheus/releases/download/v3.5.0/prometheus-3.5.0.linux-amd64.tar.gz)

然后在下载完成后，将下载的安装包上传至服务器，执行以下命令解压：

```bash
tar -zxvf prometheus-3.5.0.linux-amd64.tar.gz
```

解压完成后，按顺序执行以下指令，完成 Prometheus 服务安装：

```bash
# 进入项目文件夹
cd prometheus-3.5.0.linux-amd64

# 赋予权限
chmod -R 755 ./

# 执行（这是前台执行，会在终端中显示 prometheus 日志）
./prometheus --config.file=prometheus.yml

# 可以替换为后台执行命令（提供一个 prometheus.log 文件记录日志） 
nohup ./prometheus --config.file=prometheus.yml > prometheus.log 2>&1 & 

# 保存执行的 pid，以便后续通过 kill 进行结束 
echo $! > prometheus.pid 

# 结束运行
cat prometheus.pid
kill 10652
```

Prometheus 正常启动后，可以访问 `http://localhost:9090` 或 `http://localhost:9090/metrics` 查看系统状态（注意开启防火墙端口）。

```bash
firewall-cmd --zone=public --add-port=9090/tcp --permanent 
firewall-cmd --reload
```

![](_images/drawingbed/img/Pasted%20image%2020251211154842.png)

# Exporter 安装

Exporter 是一组程序，依据不同的设备，不同的捕获内容可以安装不同的 Exporter，官方支持的 Exporter 可以在这个链接 [ Exporters ](https://prometheus.io/docs/instrumenting/exporters/) 中查看。

不过对于大部分机器，常规的 CPU，内存，进程等占用监控可以直接访问 [ prometheus ](https://github.com/prometheus) 官方项目地址，下载其中的 node_exporter 使用。

![](_images/drawingbed/img/Pasted%20image%2020251211155208.png)

以监控 Linux 主机的 node_exporter 举例，进入项目的 [ Releases ](https://github.com/prometheus/node_exporter/releases) 页面，选择对应的版本下载，比如：[ node_exporter-1.10.2.linux-amd64.tar.gz ](https://github.com/prometheus/node_exporter/releases/download/v1.10.2/node_exporter-1.10.2.linux-amd64.tar.gz)

![](_images/drawingbed/img/Pasted%20image%2020251211155725.png)

在下载后，将文件上传至服务器，按顺序执行以下命令，启动 node_exporter 捕获程序：

```bash
# 解压文件
tar -zxvf node_exporter-1.10.2.linux-amd64.tar.gz

# 进入文件夹
cd node_exporter-1.10.2.linux-amd64

# 赋予权限
chmod -R 755 ./

# 后台执行（提供一个 node_exporter.log 文件记录日志） 
nohup ./node_exporter > node_exporter.log 2>&1 &

# 记录 pid
echo $! > node_exporter.pid 

# 结束进程
cat node_exporter.pid
ps aux | grep "node_exporter"
kill PID
```

node_exporter 在启动后会监听 9100 端口，请注意在防火墙开放，同时提供一个界面 `http://localhost:9100/metrics` 用来查看状态。

```bash
firewall-cmd --zone=public --add-port=9100/tcp --permanent 
firewall-cmd --reload
```

![](_images/drawingbed/img/Pasted%20image%2020251211161741.png)

# 在 Prometheus 中添加客户机 node_exporter

Prometheus 需要在配置文件 prometheus.yml 中添加客户机 node_exporter 的 IP 地址和端口后，才会对 node_exporter 的数据进行提取。

配置文件 prometheus.yml 应当添加的如下：

```yaml
# 全局配置
global:
  scrape_interval: 15s # 捕获间隔
  evaluation_interval: 15s # 分析间隔
  # 捕获超时可以使用参数 scrape_timeout 默认 10s
  
# 警报配置，默认不开启，需要安装警报插件
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# 加载的分析规则
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# 捕获配置
scrape_configs:
  # 工作名：这是 prometheus 的工作
  - job_name: "prometheus"
    static_configs:
      # 捕获的 IP 地址
      - targets: ["localhost:9090"]
        labels:
          app: "prometheus"

  # 这是 node_exporter 的工作
  - job_name: "node"
    static_configs:
      # 捕获的 IP 地址
      - targets: ["localhost:9100"]
```

在修改完成配置后，重新启动 Prometheus 服务，此时，Prometheus 就会按要求向 node_exporter 获取数据。

# Grafana 配置

在连接中选择 Prometheus 配置数据源。

![](_images/drawingbed/img/Pasted%20image%2020251211163855.png)

![](_images/drawingbed/img/Pasted%20image%2020251211163912.png)

![](_images/drawingbed/img/Pasted%20image%2020251211163927.png)

访问官方的 Dashboard 市场 [ Grafana Dashboards ](https://grafana.com/grafana/dashboards)，选择想要的面板。

![](_images/drawingbed/img/Pasted%20image%2020251211164115.png)

复制 ID 或者下载对应的 JSON 文件。

![](_images/drawingbed/img/Pasted%20image%2020251211164245.png)

在连接中选中我们添加的 Prometheus 数据源，点击新建面板，导入刚刚复制的 ID，或者 JSON。

![](_images/drawingbed/img/Pasted%20image%2020251211164523.png)

![](_images/drawingbed/img/Pasted%20image%2020251211164534.png)

![](_images/drawingbed/img/Pasted%20image%2020251211164630.png)

完成导入后，我们就可以看见客户机的监控数据。

![](_images/drawingbed/img/Pasted%20image%2020251211164609.png)

最后，在 Grafana 个人配置里将收刚刚创建的面板设置为首页。

![](_images/drawingbed/img/Pasted%20image%2020251211164821.png)

重新登录，或者返回 home 界面就能直接看见我们设置的面板。

![](_images/drawingbed/img/Pasted%20image%2020251211164928.png)
