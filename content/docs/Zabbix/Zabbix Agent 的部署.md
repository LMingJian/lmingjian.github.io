---
title: Zabbix Agent 的部署
date: 2024-12-25T16:17:52+08:00
author: LiangMingJian
---

# 概述

Zabbix Agent 是被部署在被监控主机上，用来主动监控本地资源（硬盘、内存、处理器等）的一个应用程序。

Zabbix Agent 会收集本地的操作信息，然后将数据报告给 Zabbix Server 用于进一步处理，当反馈数据中出现异常时，Zabbix Server 便会主动警告管理员指定机器上的异常。

# Linux 部署

```bash
rpm -Uvh https://repo.zabbix.com/zabbix/5.4/rhel/7/x86_64/zabbix-release-5.4-1.el7.noarch.rpm
yum clean all
yum install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

# 检查安装是否完成

```bash
# 服务监听
netstat -lntup|grep 10050
# 配置文件
vim /etc/zabbix/zabbix_agentd.conf
grep "^[a-Z]" /etc/zabbix/zabbix_agentd.conf
# 日志
tail -f /var/log/zabbix/zabbix_agentd.log
```

# Windows 部署

```bash
# 安装
zabbix_agentd.exe -c zabbix_agentd.win.conf -i
# 启动
zabbix_agentd.exe -s
# 卸载
zabbix_agentd.exe -d
```

——————————

> [ 下载 Zabbix ](https://www.zabbix.com/cn/download)
