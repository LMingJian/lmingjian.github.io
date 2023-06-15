---
title: Linux 防火墙 iptables 
date: 2022-06-22
author: LM
---

## 1.简介

iptables 是集成在 Linux 内核中的网络数据包过滤防火墙系统。iptables 对包的过滤遵循着 " 四表五链 "。

四表：filter 表（过滤规则表）、nat 表（地址转换规则表）、mangle 表（修改数据标记位规则表）、raw 表（跟踪数据表规则表）

- filter 表：控制数据包进出及转发，控制的链路有 INPUT、FORWARD 和 OUTPUT。
- nat 表：控制数据包中的地址转换，控制的链路有 PREROUTING、INPUT、OUTPUT 和 POSTROUTING。
- mangle 表：修改数据包中的原数据，控制的链路有 PREROUTING、INPUT、OUTPUT、FORWARD 和 POSTROUTING。
- raw 表：控制 nat 表中连接追踪机制的启用状况，控制的链路有 PREROUTING、OUTPUT。

五链：INPUT（入站数据过滤）、OUTPUT（出站数据过滤）、FORWARD（转发数据过滤）、PREROUTING（路由前过滤）和 POSTROUTING（路由后过滤），防火墙规则需要写入到这些具体的数据链中。

## 2.使用

```bash
iptables -nvL # 查看当前规则
iptables -L   # 查看当前规则
iptables -A INPUT -p tcp --dport 22 -j ACCEPT    # 开启特定端口输入
iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT   # 开启特定端口输出
iptables -L -n --line-number   # 显示规则及编号
iptables -D INPUT 2   #删除编号2的规则
```

