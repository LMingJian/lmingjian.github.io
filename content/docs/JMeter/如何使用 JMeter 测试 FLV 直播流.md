---
title: 如何使用 JMeter 测试 FLV 直播流
date: 2025-01-02T13:38:05+08:00
author: LiangMingJian
---

# 需求

使用 JMeter 完成一个 FLV 实时流的压力测试。

# 实现

FLV stands for Flash Video which supposed to be rendered and displayed by the browser plugin like this one.

When it comes to JMeter simulation it's sufficient to just download the file issuing simple single GET request using HTTP Request sampler followed by Constant Timer or Flow Control Action sampler to introduce the relevant "sleep" time to mimic the user "watching" the video.
 
FLV 代表 Flash Video，它应该由浏览器插件呈现和显示。

当使用 JMeter 模拟时，只需使用 HTTP 请求采样器发出简单的单个 GET 请求，然后使用恒定定时器或流量控制行动采样器来引入相关的“睡眠”时间来模拟用户“观看”视频就足够了。

{{< details "参考文件" >}} 
1：[ @stackoverflow ](https://stackoverflow.com/questions/71686166/load-testing-with-jmeter-for-flv-live-streaming)
{{< /details >}}
