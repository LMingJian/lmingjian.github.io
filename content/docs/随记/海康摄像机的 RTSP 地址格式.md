---
title: 海康摄像机的 RTSP 地址格式
date: 2024-12-25T10:13:27+08:00
author: LiangMingJian
---

# RTSP 取流格式

`rtsp://用户名:密码@IP:554/Streaming/Channels/101`

对于 Channels 后面的 101，其含义是第一个通道的第一条流，如果是第二通道的第一条流，则应当设置为 201，以此类推。

# 示例

取第1个通道的主码流预览：

`rtsp://admin:hik12345@10.16.4.25:554/Streaming/Channels/101`

取第1个通道的子码流预览：

`rtsp://admin:hik12345@10.16.4.25:554/Streaming/Channels/102`

取第2个通道的主码流预览：

`rtsp://admin:hik12345@10.16.4.25:554/Streaming/Channels/201`
