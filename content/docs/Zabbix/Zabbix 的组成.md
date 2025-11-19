---
title: Zabbix 的组成
date: 2024-12-25T16:17:50+08:00
author: LiangMingJian
---

# Server

Zabbix Server 是 Agents 向其报告可用性和完整性信息和统计信息的中心组件。Server 是存储所有配置、统计和操作数据的中央存储库。

# Database storage

Database 是 Zabbix 收集的所有配置信息以及数据的存储中心。
    
# Web interface

为了从任何地方和任何平台轻松访问，Zabbix 提供了基于 Web 的界面。该接口是 Zabbix Server 的一部分，通常（但不一定）与 Server 运行在同一台设备上。
    
# Proxy

Zabbix Proxy 可以代替 Zabbix Server 收集性能和可用性数据。Proxy 是 Zabbix 部署的可选部分，但是对于分散单个 Zabbix Server 的负载非常有用。
    
# Agent

Zabbix Agent 被部署在被监控目标上，可以主动监控本地资源和应用程序，并将收集到的数据报告给 Zabbix Server。
