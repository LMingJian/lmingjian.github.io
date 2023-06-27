---
title: SNI 审查
date: 2022-12-05
author: LM
---

## 1.什么是 SNI（Server Name Indication）

在很久以前，互联网上一个服务器通常只对应一个域名，一个服务器也只有一个数字证书。但是随着技术的发展，虚拟主机出现了，此时一个物理服务器上可能会管理着多个虚拟主机，多个域名，同时一个物理服务器会同时管理着多个数字证书。

因此，为了让物理服务器能区分客户端所要访问的虚拟服务器，客户端需要一个数据来标明所访问的域名，这就是**SNI（Server Name Indication）服务器名称指示**。

![](/images/drawingbed/img/202212051725289.png)

通过 Wireshark 捕获 Client Hello 包，我们可以查看到其中的 SNI 信息——sp1.baidu.com。

![](/images/drawingbed/img/202212051733397.png)

## 2.SNI 审查

SNI 审查就是通过使用上述的 SNI 信息来进行过滤，限制部分网站的访问。当客户端携带被禁止的 SNI 信息进行访问时，防火墙将会阻断该访问。由于是以域名作为过滤关键字，因此可以一次性过滤一系列网站，相较于 IP 过滤有着更高的效率。使用 SNI 审查的网站可以使用`ping`来连通网站的 IP 地址，但却无法直接访问域名。SNI 审查的原理如下：

正常情况下，TLS 握手需要经历以下几个阶段。

![](/images/drawingbed/img/202212051802167.png)

在 Client Hello 阶段，由于双方还未协商好加密方式，报文仍然是明文状态，其中 Extension 字段包含了 SNI 信息，也就包含了当前正在访问的域名，比如`steamcommunity.com`。正常情况下，服务端会根据这个域名发送相应的域名证书，进行接下来的握手步骤，在协商完后开始使用加密通信。但是，由于 SNI 已经暴漏，因此审查机构会对当中的域名进行检测和阻断，通过 TCP 重置攻击，对双方疯狂发送 RST 报文使连接重置，如果是浏览器访问就会出现`ERR_CONNECTION_RESET`错误。

现如今，大多数网站为了提升响应速度和安全等级，都会将服务交给 CDN 托管，也就是说每个 CDN 服务器都会为多家网站提供服务，例如 Steam 就将业务部署在 CDN 服务商 Akamai 上。CDN 服务器会根据用户要访问的域名提供相应的内容，也就是识别 SNI 中的域名返回相应的页面。所以 SNI 对于现在的互联网环境不可或缺。

那如何绕过审查呢？我们知道防火墙通过识别 SNI 来进行阻断，那么只要我们在 Client Hello 阶段不携带 SNI 或者携带无效的 SNI，来隐藏真实的连接网站就可以规避互联网审查。当 CDN 检测到无效的 SNI，CDN 就会返回一张默认证书来保证能够继续连接。碰巧的是，Akamai 所返回的默认证书就是 Steam 的泛域名证书（毕竟 Steam 是最大的客户）。这种技术就叫域前置。

## 3.绕过

通过伪造错误的 SNI 或直接隐藏 SNI，可以绕过 SNI 审查。当服务器检测到无效的 SNI 时，往往会返回一张默认证书以便能够继续连接，这种技术叫做域前置。我们这里使用 nginx，利用其反向代理的功能来尝试绕过 SNI 审查访问 Pixiv 首页。

- 在 nginx 上配置一个服务器，打开 nginx 目录下 conf/nginx.conf 文件，添加如下配置

```nginx
 server {
        listen       443 ssl;
        server_name  www.pixiv.net;

        # 自签一个数字证书，下面为其公钥和私钥
        ssl_certificate      cert.pem;
        ssl_certificate_key  cert.key;

        location / {
            proxy_pass https://210.140.131.220:443;
            proxy_set_header Host $http_host;
        }
```

- 上述代码完成了反向代理 Pixiv 的配置。目的是将一个 host 为 `www.pixiv.net` 的网站代理到 `210.140.131.220`，其中 `210.140.131.220` 为 Pixiv 可访问的 IP 地址。
- 使用 POSTMAN 验证反向代理成果。

![](/images/drawingbed/img/202212051752581.png)

- 成功绕过审查，Windows 结束 Nginx 服务：`taskkill /f /t /im nginx.exe`

## 4.相关术语

#### HTTP（Hyper Text Transfer Protocol）

超文本传输协议，日常浏览的各种网页都是通过 HTTP 协议传输，但是它的传输过程没有加密，不安全，所以被 HTTPS 替代。目前市面上所说的支持 HTTP 一般是指 HTTP 和 HTTPS 功能都可以实现，而不是只支持 HTTP。

#### TLS（Transport Layer Security）

安全传输层，它的前身是 SSL，它实现了将报文加密后再交由 TCP 进行传输的功能。即 HTTP + TLS = HTTPS。

#### SNI（Server Name Indication）

服务器名称指示，简单来说，是用于在一台服务器的相同端口上部署不同证书的方法。服务器根据收到请求中的 SNI 域名来处理相应的请求，如果 SNI 域名为空，会按照预先设置好的默认域名处理请求。

#### CDN（Content Delivery Network）

内容分发网络，各个地区部署的服务器在一起形成的高速网络拓扑，用户在每个地区都能实现快速访问。CDN 通过 SNI 响应不同的网站，同时也保护根服务器的安全。

#### 反向代理

在根服务器前部署一台外层服务器做为网关，用户只能访问到外层服务器，从而保护内网服务器不被暴露。做为网关，也可以对报文分析和修改。代理服务器和根服务器部署在一台主机上称为本地反代。

{{< details "参考文件" >}} 
1：[ 绕过 SNI 审查 @bubuko ](http://www.bubuko.com/infodetail-3630653.html)

2：[ 从 Steam 社区屏蔽分析绕过方法及 ASF 安全部署 @NiGhT_Ray ](https://www.cnblogs.com/night-ray/articles/15964334.html)

3：[ PIXIV网页版及客户端访问恢复指南 @樱花庄的白猫 ](https://2heng.xin/2017/09/19/pixiv/)
{{< /details >}}