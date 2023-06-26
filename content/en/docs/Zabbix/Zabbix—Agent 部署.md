---
title: Zabbix-Agent 数据收集器的部署
date: 2022-01-20
author: LM
---

## 1.部署（环境 CentOS7）

Zabbix 通过部署在被监控机上的 Zabbix-Agent 进行数据收集。Zabbix-Agent 在机器上收集并整理数据后会传递给 Zabbix 服务器，交由其处理展示。

```bash
rpm -Uvh https://repo.zabbix.com/zabbix/5.4/rhel/7/x86_64/zabbix-release-5.4-1.el7.noarch.rpm
yum clean all
yum install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

## 2.检查

```bash
# 服务监听
netstat -lntup|grep 10050
# 配置文件
vim /etc/zabbix/zabbix_agentd.conf
grep "^[a-Z]" /etc/zabbix/zabbix_agentd.conf
# 日志
tail -f /var/log/zabbix/zabbix_agentd.log
```

## 3.部署（环境 Windows）

```bash
# 安装
zabbix_agentd.exe -c zabbix_agentd.win.conf -i
# 启动
zabbix_agentd.exe -s
# 卸载
zabbix_agentd.exe -d
```

{{< details "参考文件" >}} 
1：[下载Zabbix](https://www.zabbix.com/cn/download)
{{< /details >}}