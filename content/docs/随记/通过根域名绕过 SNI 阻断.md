---
title: 通过根域名绕过 SNI 阻断
date: 2026-10-20T15:43:11+08:00
author: LiangMingJian
---

# 解决方案

1. 套 CDN ，使用是 Cloudflare 的 CDN 。

2. HTTP/3+DOH+DNS 解析添加 HTTPS 类型的解析，直接走 QUIC 协议。

3. 直接在域名后面加上根域名 `.` 然后访问，比如需要访问的域名是 `example.com`，直接访问 `example.com.` 这个地址可以绕过阻断，目前原理不清楚。
