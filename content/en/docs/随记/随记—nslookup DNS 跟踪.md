---
title: DNS 跟踪命令 nslookup 
date: 2023-03-16
author: LM
---

## 1.定义

nslookup，全称 name server lookup，通常用于查询 DNS 的记录，检查域名解析是否正常，诊断 DNS 基础结构的信息。

## 2.非交互模式使用

通过 `nslookup 域名` 可以查询相应的 DNS 记录。

```
PS > nslookup baidu.com
服务器:  public1.alidns.com
Address:  223.5.5.5

非权威应答:
名称:    baidu.com
Addresses:  39.156.66.10
          110.242.68.66
```

## 3.交互模式使用

通过 `nslookup` 可以进入交互模式，在结束终端前可以一直查询对应的域名信息。

```
PS > nslookup
默认服务器:  public1.alidns.com
Address:  223.5.5.5

> baidu.com
服务器:  public1.alidns.com
Address:  223.5.5.5

非权威应答:
名称:    baidu.com
Addresses:  39.156.66.10
          110.242.68.66

>
```

## 4.一些参数

反向 DNS 解析，使用参数 `-ty=ptr IP`

```
PS > nslookup -ty=ptr 8.8.8.8
服务器:  public1.alidns.com
Address:  223.5.5.5

非权威应答:
8.8.8.8.in-addr.arpa    name = dns.google
```

帮助手册，在交互模式中输入`？`

```
PS > nslookup
默认服务器:  public1.alidns.com
Address:  223.5.5.5

> ?
命令:   (标识符以大写表示，[] 表示可选)
NAME            - 打印有关使用默认服务器的主机/域 NAME 的信息
NAME1 NAME2     - 同上，但将 NAME2 用作服务器
help or ?       - 打印有关常用命令的信息
set OPTION      - 设置选项
    all                 - 打印选项、当前服务器和主机
    [no]debug           - 打印调试信息
    [no]d2              - 打印详细的调试信息
    [no]defname         - 将域名附加到每个查询
    [no]recurse         - 询问查询的递归应答
    [no]search          - 使用域搜索列表
    [no]vc              - 始终使用虚拟电路
    domain=NAME         - 将默认域名设置为 NAME
    srchlist=N1[/N2/.../N6] - 将域设置为 N1，并将搜索列表设置为 N1、N2 等
    root=NAME           - 将根服务器设置为 NAME
    retry=X             - 将重试次数设置为 X
    timeout=X           - 将初始超时间隔设置为 X 秒
    type=X              - 设置查询类型(如 A、AAAA、A+AAAA、ANY、CNAME、MX、
                          NS、PTR、SOA 和 SRV)
    querytype=X         - 与类型相同
    class=X             - 设置查询类(如 IN (Internet)和 ANY)
    [no]msxfr           - 使用 MS 快速区域传送
    ixfrver=X           - 用于 IXFR 传送请求的当前版本
server NAME     - 将默认服务器设置为 NAME，使用当前默认服务器
lserver NAME    - 将默认服务器设置为 NAME，使用初始服务器
root            - 将当前默认服务器设置为根服务器
ls [opt] DOMAIN [> FILE] - 列出 DOMAIN 中的地址(可选: 输出到文件 FILE)
    -a          -  列出规范名称和别名
    -d          -  列出所有记录
    -t TYPE     -  列出给定 RFC 记录类型(例如 A、CNAME、MX、NS 和 PTR 等)
                   的记录
view FILE           - 对 'ls' 输出文件排序，并使用 pg 查看
exit            - 退出程序

>
```