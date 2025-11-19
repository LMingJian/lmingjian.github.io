---
title: Zabbix 的监控模式
date: 2024-12-25T16:17:47+08:00
author: LiangMingJian
---

# 被动模式

**被动模式是 Zabbix 默认使用的监控模式**

在被动模式中，由 Zabbix Server 向 Agent 发起连接，并发送监控 Key，Agent 在收到请求后，响应监控数据。

这种模式被称为被动监控，其特点是由服务器轮询监控主机，获取数据。

# 主动模式

在主动模式中，由 Agent 向 Zabbix Server 发起连接，向服务器请求需要检测的监控项目列表，然后由 Zabbix Server 响应并向 Agent 发送一个 Items 列表，Agent 在收到监控列表后开始周期性地收集数据，并发送给 Zabbix Server。

在主动模式中，Zabbix Server 不用每次需要数据时都连接 Agent，Agent 会自己收集并处理数据发送给 Zabbix Server，Zabbix Server 仅需要保存数据即可。

这种模式被称为主动监控，其特点是由监控主机自己收集统计并发送给服务器，服务器不做轮询操作。

{{< details "参考文件" >}} 
1： [ Zabbix 使用手册 ](https://www.zabbix.com/documentation/current/zh/manual)
{{< /details >}}
