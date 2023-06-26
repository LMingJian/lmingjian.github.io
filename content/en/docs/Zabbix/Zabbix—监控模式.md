---
title: Zabbix 的监控模式
date: 2022-01-20
author: LM
---

## 1.被动监控（默认）

Zabbix Server 向 Agent 发起连接，发送监控 Key，由 Agent 接受请求，响应监控数据。这种模式被称为被动监控，其特点是由服务器轮询监控主机，获取数据。

## 2.主动监控

Agent 向 Zabbix Server 发起连接，Agent 请求需要检测的监控项目列表，Zabbix Server 响应并向 Agent 发送一个 items 列表，Agent 在收到监控列表后开始周期性地收集数据，并发送给 Zabbix Server，这样 Zabbix Server 不用每次需要数据都连接 Agent，Agent 会自己收集并处理数据，Zabbix Server 仅需要保存数据即可。这种模式被称为主动监控，其特点是由监控主机自己收集统计并发送给服务器，服务器不做询问操作。

