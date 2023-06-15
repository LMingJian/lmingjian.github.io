---
title: Linux 防火墙 firewall-cmd
date: 2020-10-16
author: LM
---

## 1.防火墙操作

在 Linux 中，有大部分发行版使用 firewall-cmd 进行防火墙控制。

```bash
cd /etc/firewalld/zones # 查看防火墙文件
firewall-cmd --zone=public --add-port=8080/tcp --permanent # 开放8080/tcp端口 （--permanent永久生效，没有此参数重启后失效）
firewall-cmd --zone=public --remove-port=8080/tcp --permanent # 关闭8080/tcp端口
firewall-cmd --zone=public --query-port=80/tcp # 查看端口状态
firewall-cmd --reload # 重启防火墙
firewall-cmd --completely-reload
firewall-cmd --list-all # 列出防火墙所以规则
firewall-cmd --list-ports # 列出防火墙开放的端口
firewall-cmd --version # 查看版本： 
firewall-cmd --help
firewall-cmd --state
firewall-cmd --panic-on # 拒绝所有包：
firewall-cmd --panic-off # 取消拒绝状态： 
firewall-cmd --query-panic # 查看是否拒绝： 
firewall-cmd --zone=docs --add-port=40000-45000/tcp --permanent # 批量开放端口，打开从40000到45000之间的所有端口
firewall-cmd --zone=docs --remove-port=40000-45000/tcp --permanent # 批量关闭端口，关闭从40000到45000之间的所有端口
firewall-cmd --zone=work --add-service=smtp  # 添加服务
firewall-cmd --zone=work --query-service=smtp  # 查看服务
firewall-cmd --zone=work --remove-service=smtp  # 删除服务
#-------------------------------------------------------
systemctl stop firewalld # 关闭防火墙
systemctl start firewalld # 开启防火墙
systemctl status firewalld # 防火墙状态
systemctl stop firewalld # 停止
systemctl disable firewalld # 开机禁用
systemctl enable firewalld # 开机启动
yum install firewalld # 安装
```
