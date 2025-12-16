---
title: 如何用 Grafana + Loki 搭建日志监控系统
date: 2024-12-25T10:32:02+08:00
author: LiangMingJian
---

# 监控系统组成

通过搭建 Grafana，Loki 以及 Promtail 来建立日志监控系统。

**Grafana** 是由 Grafana Labs 公司开源的一个监控仪表系统。它可以帮助用户简化监控的复杂度，用户只需提供数据，便可以生成各种可视化仪表。同时还支持报警功能，可以在系统出现问题时通知用户。

**Loki** 是 Grafana Lab 公司开源的一个水平可扩展、高可用性、多租户的日志聚合系统。提供对日志的收集，建立标签索引的功能，实现对日志的监控。

**Promtail** 是被监控机向 Loki 推送监控样本数据的一个工具。Loki 不会主动的监控日志，它仅做收集功能，因此，为了能够监控日志，需要在被监控机上使用 Promtail，由 Promtail 周期性的向 Loki 暴露的 HTTP 服务地址推送监控样本数据。

Grafana，Loki 以及 Promtail 的系统结构如下：

![](/_images/drawingbed/img/202206221443693.png)

Grafana，Loki 以及 Promtail 的对应的官方网站如下：

[ Grafana Download ](https://grafana.com/grafana/download)

[ Install Loki ](https://grafana.com/docs/loki/latest/setup/install/)

[ Promtail Releases ](https://github.com/grafana/loki/releases)

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

# Loki 的安装

Loki 的安装与 Grafana 一样，同样使用二进制源代码进行安装。

访问 Loki 的仓库 [ Loki Release ](https://github.com/grafana/loki/releases/)，下载所需版本的安装包，比如：[ loki-linux-amd64.zip ](https://github.com/grafana/loki/releases/download/v3.6.2/loki-linux-amd64.zip)。

> 注意，请不要下载 LogCLI 和 Loki Canary，这两个并不是安装包。

![](_images/drawingbed/img/Pasted%20image%2020251210175952.png)

在下载完成后，上传安装包到服务器，执行以下命令解压：

```bash
unzip loki-linux-amd64.zip
```

解压完成后，按顺序执行以下指令，完成 Loki 服务安装：

```bash
# 下载默认的配置文件
wget https://raw.githubusercontent.com/grafana/loki/main/cmd/loki/loki-local-config.yaml

# 赋予权限
chmod 755 loki-linux-amd64

# 执行（这是前台执行，会在终端中显示 Loki 日志）
./loki-linux-amd64 -config.file=loki-local-config.yaml

# 可以替换为后台执行命令（提供一个 loki.log 文件记录日志）
nohup ./loki-linux-amd64 -config.file=loki-local-config.yaml > loki.log 2>&1 &

# 保存执行的 pid，以便后续通过 kill 进行结束
echo $! > loki.pid

# 结束运行
cat loki.pid
kill 9842
```

在上述操作完成后，Loki 就成功启动。

Loki 默认的访问端口为 3100（请注意开放端口），同时提供一个网页以查看状态：`http://localhost:3100/metrics`。

```bash
firewall-cmd --zone=public --add-port=3100/tcp --permanent
firewall-cmd --reload
```

![](_images/drawingbed/img/Pasted%20image%2020251210185632.png)

# Promtail 安装

Promtail 与 Loki 是配套使用的一套工具，Loki 通过 Promtail 获取日志数据，缺少 Promtail，Loki 将无法对日志进行统计分析。

Promtail 的仓库与 Loki 是同一个，因此可以前往：[ Loki Releases ](https://github.com/grafana/loki/releases) 进行下载，比如：[ promtail-linux-amd64.zip ](https://github.com/grafana/loki/releases/download/v3.6.2/promtail-linux-amd64.zip)

![](_images/drawingbed/img/Pasted%20image%2020251210193255.png)

在下载完成后，上传安装包到服务器，执行以下命令解压：

```bash
unzip promtail-linux-amd64.zip
```

解压完成后，按顺序执行以下指令，完成 Promtail 服务安装：

```bash
# 下载 promtail 配置文件
wget https://raw.githubusercontent.com/grafana/loki/main/clients/cmd/promtail/promtail-local-config.yaml

# 赋予权限
chmod 755 promtail-linux-amd64

# 执行（这是前台执行，会在终端中显示 Loki 日志）
./promtail-linux-amd64 -config.file=promtail-config.yaml

# 可以替换为后台执行命令（提供一个 loki.log 文件记录日志）
nohup ./promtail-linux-amd64 -config.file=promtail-config.yaml > promtail.log 2>&1 &

# 保存执行的 pid，以便后续通过 kill 进行结束
echo $! > promtail.pid

# 结束运行
cat promtail.pid
kill 9842
```

> 如果新版 promtail 在部分过旧服务器中无法运行，出现 `/lib64/libc.so.6: version GLIBC_2.32 not found` 的错误，则建议用户将 promtail 替换为旧版本使用。注意降级的同时需要检测配置文件是否符合这个版本。

另外，对于 Loki 服务器不在与 promtail 同一个的服务器，promtail 应当将 `promtail-config.yaml` 配置文件中的 `clients` 替换为目标 Loki 服务器。

`promtail-config.yaml` 配置文件

```yml
# 服务器配置
server:
  http_listen_port: 9080
  grpc_listen_port: 0

# 文件偏移量的存储文件路径
positions:
  filename: /tmp/positions.yaml

# Loki 的 API 地址
clients:
  - url: http://localhost:3100/loki/api/v1/push

# 捕获配置
scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs  # 日志任务标签
      __path__: /var/log/*log  # 日志路径
      stream: stdout  # 日志流标识
```

# Grafana 配置

在 Grafana 中选择 Loki 数据源，进行 Explore 即可对日志进行查找。

![](_images/drawingbed/img/Pasted%20image%2020251210195720.png)

![](_images/drawingbed/img/Pasted%20image%2020251210195744.png)

![](_images/drawingbed/img/Pasted%20image%2020251210195812.png)

————————————

> 详细的操作手册请查看文档：[ Grafana Loki ](https://grafana.com/docs/loki/latest/?pg=oss-loki&plcmt=quick-links) 或 [ Grafana OSS and Enterprise ](https://grafana.com/docs/grafana/latest/)
