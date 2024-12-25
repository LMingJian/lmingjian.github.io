---
title: 如何使用 InfluxDB2 监听 JMeter 测试数据
date: 2024-12-25T13:54:17+08:00
author: LiangMingJian
---

# 概述

在使用 JMeter 进行性能测试时，往往需要将 JMeter 测试生成的数据进行可视化处理，将数据结果转换为图表，以便后续进行分析。这个过程，我们可以使用 InfluxDB 来进行处理。注意，以下内容仅适用于 InfluxDB 2.0 版本。

# InfluxDB2 部署

```
docker pull influxdb
docker run -d --name jmeter-influx -p 8083:8083 -p 8086:8086 influxdb
```

访问`IP:8086`，可以进行登录，以及系统配置。

# 配置 InfluxDB2

1. 创建一个你的组织
2. 创建一个名为`jmeter`的`bucket`
3. 创建一个`API Token`

# 配置 JMeter

1. 创建一个 `Backend Listener`
2. 选择`Backend Listener implementation`，然后填入以下内容：

```
org.apache.jmeter.visualizers.backend.influxdb.influxdbBackendListenerClient
```

3. 在`Backend Listener`中，找到并填写以下内容：

- influxdbMetricsSender：

```
org.apache.jmeter.visualizers.backend.influxdb.HttpMetricsSender
```

- influxdbUrl：

```
http://localhost:8086/api/v2/write?org=my-org&bucket=jmeter
```

- application：`InfluxDB2`
- influxdbToken：`your InfluxDB API token`

4. 启动测试后，数据会自动发送给 InfluxDB2 进行可视化展示

{{< details "参考文件" >}} 
1：[ InfluxDB OSS 2.3 Documentation ](https://docs.influxdata.com/influxdb/v2.3/write-data/no-code/third-party/#configure-apache-jmeter)
{{< /details >}}
