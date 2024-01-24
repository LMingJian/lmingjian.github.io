---
title: Cloudflare 的人机验证流程
date: 2023-08-11
author: LM
---

## 1.人机验证流程

Cloudflare 的人机验证 Captcha 页面基本是依赖 JS 实现。

首先 Cloudflare Captcha 页面会加载一段 JS 文件，当用户解决验证以后，这个 JS 会发送一个 POST 请求，向服务器获取 Cookie。

Cloudflare 在收到 POST 请求后，会给这个 POST 请求返回原始请求的内容，同时传递一个 `cf_clearance` 的 Cookie，在有效期 24 小时内，用户可以使用 Cookie 不进行验证的访问，

需要注意的是 Cloudflare 对于使用 `cf_clearance` 的请求有如下几点要求，只有满足这三个需求，才能使用 `cf_clearance` 通过 Cloudflare 了。

1. `cf_clearance`还在有效期内。
2. `User-Agent`要一致。
3. IP 要在同一个 C 段内，但不要求同一个 IP。