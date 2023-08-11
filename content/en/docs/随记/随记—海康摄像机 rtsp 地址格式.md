---
title: 海康摄像机 rtsp 地址格式
date: 2020-09-19
author: LM
---

## 1.RTSP 单播取流格式

`rtsp://用户名:密码@IP:554/Streaming/Channels/101`

## 2.示例

取第1个通道的主码流预览：`rtsp://admin:hik12345@10.16.4.25:554/Streaming/Channels/101`

取第1个通道的子码流预览：`rtsp://admin:hik12345@10.16.4.25:554/Streaming/Channels/102`

取第2个通道的主码流预览：`rtsp://admin:hik12345@10.16.4.25:554/Streaming/Channels/201`

## 3.多播取流格式

`rtsp://用户名:密码@IP:554/Streaming/Channels/101?transportmode=unicast`

## 4.示例

取第1个通道的主码流预览：`rtsp://admin:hik12345@10.16.4.25:554/Streaming/Channels/101?transportmode=unicast`

{{< details "参考文件" >}} 
1：[ 海康摄像机 rtsp 地址格式汇总 @牛哥 ](https://zhuanlan.zhihu.com/p/151377691)
{{< /details >}}

