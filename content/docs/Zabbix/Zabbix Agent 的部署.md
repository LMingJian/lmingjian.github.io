---
title: Zabbix Agent 的部署
date: 2024-12-25T16:17:52+08:00
author: LiangMingJian
---

# 概述

**Zabbix Agent 部署在被监控目标上，以主动监控本地资源和应用程序（硬盘、内存、处理器统计信息等）。**

Zabbix Agent 收集本地的操作信息并将数据报告给 Zabbix Server 用于进一步处理。一旦出现异常 (例如硬盘空间已满或者有崩溃的服务进程)，Zabbix Server 会主动警告管理员指定机器上的异常。

Zabbix Agents 的极高效率缘于它可以利用本地系统调用来完成统计数据的采集。

# Linux 部署

**测试环境：CentOS 7.9**

```
rpm -Uvh https://repo.zabbix.com/zabbix/5.4/rhel/7/x86_64/zabbix-release-5.4-1.el7.noarch.rpm
yum clean all
yum install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

# 检查安装是否完成

**测试环境：CentOS 7.9**

```
# 服务监听
netstat -lntup|grep 10050
# 配置文件
vim /etc/zabbix/zabbix_agentd.conf
grep "^[a-Z]" /etc/zabbix/zabbix_agentd.conf
# 日志
tail -f /var/log/zabbix/zabbix_agentd.log
```

# Windows 部署

```
# 安装
zabbix_agentd.exe -c zabbix_agentd.win.conf -i
# 启动
zabbix_agentd.exe -s
# 卸载
zabbix_agentd.exe -d
```

{{< details "参考文件" >}} 
1：[ 下载 Zabbix ](https://www.zabbix.com/cn/download)
{{< /details >}}
