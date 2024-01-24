---
title: Zabbix 及其监控模式
date: 2022-01-20
author: LM
weight: 3001
---

## 1.什么是 Zabbix

Zabbix 是由 Alexei Vladishev 创建，目前由 Zabbix SIA 主导开发和支持的一个企业级，开源的，分布式资源监控解决方案。

Zabbix 由下面几个主要的软件组件所组成：

- Server：Zabbix Server 是 Agents 向其报告可用性和完整性信息和统计信息的中心组件。Server 是存储所有配置、统计和操作数据的中央存储库。
- Database：Zabbix 收集的所有配置信息以及数据都存储在数据库中。
- Web：为了从任何地方和任何平台轻松访问，Zabbix 提供了基于 Web 的界面。该接口是 Zabbix Server 的一部分，通常（但不一定）与 Server 运行在同一台设备上。
- Proxy：Zabbix Proxy 可以代替 Zabbix Server 收集性能和可用性数据。Proxy 是 Zabbix 部署的可选部分；但是对于分散单个 Zabbix Server 的负载非常有用。
- Agent：Zabbix Agent 部署在被监控目标上，可以主动监控本地资源和应用程序，并将收集到的数据报告给 Zabbix Server。

Zabbix 是一款监控网络众多参数以及服务器、虚拟机、应用程序、服务、数据库、网站、云等健康和完整性的软件。

## 2.Zabbix 的监控模式

### 2.1 被动监控（默认）

Zabbix Server 向 Agent 发起连接，发送监控 Key，由 Agent 接受请求，响应监控数据。

这种模式被称为被动监控，其特点是由服务器轮询监控主机，获取数据。

### 2.2 主动监控

Agent 向 Zabbix Server 发起连接，请求需要检测的监控项目列表，Zabbix Server 响应并向 Agent 发送一个 items 列表，Agent 在收到监控列表后开始周期性地收集数据，并发送给 Zabbix Server，这样 Zabbix Server 不用每次需要数据都连接 Agent，Agent 会自己收集并处理数据，Zabbix Server 仅需要保存数据即可。

这种模式被称为主动监控，其特点是由监控主机自己收集统计并发送给服务器，服务器不做询问操作。

{{< details "参考文件" >}} 
1：[ Zabbix 使用手册 ](https://www.zabbix.com/documentation/current/zh/manual)
{{< /details >}}