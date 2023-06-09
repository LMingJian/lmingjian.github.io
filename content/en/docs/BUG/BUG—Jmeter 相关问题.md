---
title: Jmeter 相关问题
date: 2020-11-28
author: LMingJian
---

## BUG 描述

运行内存配置修改。

## Resolution

修改 `bat` 文件中 `HEAP` 值。`HEAP=-Xms**5g** -Xmx**5g**`，最小与最大运行内存。`MaxMetaspaceSize`，最大堆栈 。

```shell
if not defined HEAP (
    rem See the unix startup file for the rationale of the following parameters,
    rem including some tuning recommendations
    set HEAP=-Xms5g -Xmx5g -XX:MaxMetaspaceSize=5120m
)
```

------

## BUG 描述

端口被突然关闭 ，提示`socket closed`。

## Resolution

原因：发送请求时，Jmeter 一般默认选择 `Use KeepAlive`，即保持连接协议，但由于 `JMeter.properties` 中时间设置默认注销，即不会等待，因此一旦连接空闲，就会断开，从而导致报错。

解决：修改 `httpclient4.idletimeout=<time in ms>` ，一般可设置成 `1000-6000ms`（表示连接空闲10s后才会断开）。

------

## BUG 描述

地址被占用 `address already in use:connect`。

错误：`java.net.BindException: Address already in use: connect`。

## Resolution

原因：Windows 端口被耗尽（默认1024-5000），且短时间内这些端口不会释放，Windows 端口最大数为 65534。

解决：修改操作系统注册表(`cmd > regedit`)，找到 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\TCPIP\Parameters` ，新建两个 `DWORD` 值，`{MaxUserPort：65534, TcpTimedWaitDelay：30}`，重启系统。

【或设置线程组时，勾选 `same user on each iteration`】 

【或不勾选 `Use KeepAlive`】