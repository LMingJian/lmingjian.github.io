---
title: 如何在 Linux 中搭建 JMeter
date: 2024-12-25T13:54:32+08:00
author: LiangMingJian
---

# 部署

在 Linux 下搭建 Jmeter 性能测试环境。

```shell
tar -xvf Jmeter.tgz # 解压文件
vi /etc/profile # 编辑 Jmeter 环境
# Jmeter environment
export JMETER_HOME=/root/Jmeter
export PATH=$PATH:${JMETER_HOME}/bin
#######
source /etc/profile # 启动环境
jmeter -v # 检查是否成功运行
jmeter –n –t test.jmx –l test.jtl # 执行 jmeter 脚本
```

# 支持的参数

```
-h 帮助 -> 打印出有用的信息并退出
-n 非 GUI 模式 -> 在非 GUI 模式下运行 JMeter
-t 测试文件 -> 要运行的 JMeter 测试脚本文件
-l 日志文件 -> 记录结果的文件
-r 远程执行 -> 启动远程服务
-H 代理主机 -> 设置 JMeter 使用的代理主机
-P 代理端口 -> 设置 JMeter 使用的代理主机的端口号
```
