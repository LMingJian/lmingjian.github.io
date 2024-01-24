---
title: 什么是 Socket.IO
date: 2021-11-23
author: LM
---

## 1.工作原理

Socket.IO 是 Websocket 的一个实现，其分为服务器（node.js）和客户端（浏览器、node.js或其他编程语言）。服务器与客户端之间的双向通道使用 Websocket 连接建立，并使用 HTTP 长轮询作为回退。

Socket.IO 代码库分为两个不同的层：

- 低级管道：称为 Engine.IO，作为 Socket.IO 的内部发动机，负责建立服务器和客户端之间的低级连接，处理各种数据运输，升级机制，以及断开检测。
- 高级别 API： Socket.IO 本身

## 2.连接过程

### b.握手

在 Engine.IO 连接的开始，服务器会发送一些信息：

```json
{  
    "sid": "FSDjX-WRwSA4zTZMALqx",  
    "upgrades": ["websocket"],  
    "pingInterval": 25000,  
    "pingTimeout": 20000
}
```

- `sid` 是会话的 ID，它必须包含在所有后续 HTTP 请求中的查询参数中
- `upgrades` 包含由服务器支持的所有链接列表
- `pingInterval` 与 `pingTimeout` 的值用于心跳机制，以检查连接状态

### c.升级机制

默认情况下，客户端会先与 HTTP 长轮询传输建立连接。

虽然 Websocket 是建立双向通信的最佳方式，但经验表明，由于代理、防火墙、防病毒软件等原因，建立 Websocket 连接并不总是可能的。从用户的角度来看，不成功的 Websocket 连接会伤害用户体验。

综上所及，Engine.IO 首先关注可靠性和用户体验，其次再改进和提高服务器性能。

Engine.IO 可以将 HTTP 长轮询升级为 Websocket，升级时，客户端将：

- 确保其传出缓冲器是空的
- 将当前传输置于仅读模式
- 尝试与其他运输建立连接
- 如果成功，关闭第一次运输

您可以查看浏览器的网络监视器：

![](/images/drawingbed/img/202204291745285.png)

1. 握手 （包含会话 ID `sid = zBjrh...AAAK` 在此处 ，在随后的请求中使用）
2. 发送数据（HTTP 长轮询）
3. 接收数据（HTTP 长轮询）
4. 升级（Websocket）
5. 接收数据（HTTP 长轮询会在成功建立 Websocket 连接后关闭）

### d.断开检测

Engine.IO 连接会在下列情况中视为关闭：

- 一个 HTTP 请求（GET 或 POST）失败
- 网络插座连接已关闭（例如，当用户关闭浏览器中的选项卡时）
- `socket.disconnect()`在服务器端或客户端调用

还有一个心跳机制，检查服务器和客户端之间的连接是否仍然启动和运行：

在给定间隔，服务器发送 PING 数据包，客户端将 PONG 数据包发回。如果服务器没有收到 PONG 数据包，它将认为连接已关闭。相反，如果客户端内部未收到 PING 数据包，则会认为连接已关闭。

{{< details "参考文件" >}} 
1：[ Introduction @Socket.IO ](https://socket.io/docs/v4/how-it-works/)
{{< /details >}}