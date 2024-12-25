---
title: 修复 Flutter Dio 拒绝 HTTP 连接的问题
date: 2024-12-25T11:20:59+08:00
author: LiangMingJian
---

# 报错

在使用 Dio 进行网络连接的 Flutter 项目中，若生成的应用在真机运行，则会出现如下错误：`DioError [DioErrorType.DEFAULT]: Bad state: Insecure HTTP is not allowed by platform`。

# 原因

平台不支持不安全的 HTTP 协议，即不允许访问 HTTP 域名的地址。这是因为 IOS 和 Android 9.0 对网络请求做了一些限制，不能直接访问 HTTP 域名的地址。

# 解决方案

降低 SDK 版本至 27 或 27 以下。

{{< details "参考文件" >}} 
1：[ Insecure HTTP is not allowed by platform @csdn ](https://blog.csdn.net/weixin_44137575/article/details/109045633)
{{< /details >}}
