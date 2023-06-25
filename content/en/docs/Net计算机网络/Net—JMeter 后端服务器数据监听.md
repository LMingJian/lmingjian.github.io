---
title: JMeter 后端服务器数据监听
date: 2022-07-29
author: LM
---

## 1.简介

在使用 JMeter 进行性能测试时，往往需要将 JMeter 测试生成的数据进行可视化处理，将数据结果转换为图表，以便后续进行分析。这个过程，我们可以使用 InfluxDB 来进行处理。注意，以下内容仅适用于 InfluxDB 2.0 版本。

## 2.在 InfluxDB2

1. 创建一个你的组织
2. 创建一个名为`jmeter`的`bucket` 
3. 创建一个`API Token`

## 3.在 JMeter

1. 创建一个 `Backend Listener`

2. 选择`Backend Listener implementation`

   ```
   org.apache.jmeter.visualizers.backend.influxdb.influxdbBackendListenerClient
   ```

3. 填写以下数据

   - influxdbMetricsSender

     ```
     org.apache.jmeter.visualizers.backend.influxdb.HttpMetricsSender
     ```

   - influxdbUrl

     ```
     http://localhost:8086/api/v2/write?org=my-org&bucket=jmeter
     ```

   - application: `InfluxDB2`

   - influxdbToken: `your InfluxDB API token`

4. 启动测试，数据便会发送给 InfluxDB

## 4.InfluxDB2 部署

```
docker pull influxdb
docker run -d --name jmeter-influx -p 8083:8083 -p 8086:8086 influxdb
```

部署完成后，访问`IP:8086`，登录并配置

{{< details "参考文件" >}} 
1：[InfluxDB OSS 2.3 Documentation (influxdata.com)](https://docs.influxdata.com/influxdb/v2.3/write-data/no-code/third-party/#configure-apache-jmeter)
{{< /details >}}