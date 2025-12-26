---
title: 什么是 CORS？OPTIONS 预检请求有什么用？
date: 2024-12-25T10:33:08+08:00
author: LiangMingJian
---

# 什么是 CORS？

CORS，Cross-Origin Resource Sharing，跨域资源共享，是一种基于 HTTP 头，允许浏览器向**跨源**服务器发送 XMLHttpRequest 和 Fetch API，即我们常说的 HTTP 请求的机制。

> 一个跨源的 HTTP 请求：某个运行在 `https://example.com` 的 JavaScript 代码使用 XMLHttpRequest 来发起一个到 `https://api.example.org` 的请求。

在浏览器中，出于安全性，其一般限制 JavaScript 脚本内所有发起的跨源 HTTP 请求，即浏览器只允许 JavaScript 向同源的 Web 服务器发送请求（这种行为称为同源安全策略）。正因如此，CORS 的出现为浏览器访问其他服务器提供了实现基础。

> 在同源安全策略中，包括请求协议不同（http，https 也算），域不同，端口不同在内的这些情况都属于**跨域**。浏览器只信任当前域的资源，而对其他域的资源表示不信任，比如浏览器成功访问了 `https://example.com`，但因为同源安全策略，所以浏览器无法直接访问 `https://example.com:8080`，即使这两个域名都是同一个网站的内容。

整个 CORS 通信过程，都是由浏览器自动完成，完全不需要用户参与。对于开发者来说，CORS 通信与同源的 AJAX 通信没有差别，代码完全一样。浏览器一旦发现 AJAX 请求跨源，就会自动添加一些附加的头信息（Origin），或多出一次附加的请求（OPTIONS），但用户不会有感觉。

> AJAX：Asynchronous JavaScript and XML，异步的 JavaScript 和 XML，常用来执行 HTTP 请求的 JavaScript 代码。

# 头信息 Origin

当用户通过 JavaScript 发出 HTTP 请求时，浏览器会自动判断这个请求是不是跨域的。

**如果发现该浏览器跨域，那么浏览器便会调用 CORS 机制，在头信息之中，增加一个 `Origin` 字段，标识这个请求是从什么地方来的，要求服务器进行确认**。

比如下面的 GET 请求，有 `Origin` 字段 `http://api.bob.com`，表示请求从这个地方来的，服务器要根据这个值，决定是否同意这次请求。

```http
GET /cors HTTP/1.1
Origin: http://api.bob.com
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

**服务器如果判断 Origin 指定的域名在许可范围内，那么服务器就会在返回的响应中添加几个特殊的头信息字段。**

比如下面的响应头：

```http
GET /cors HTTP/1.1
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```

上面的头信息之中，有三个是与 CORS 请求相关的字段，他们都以 `Access-Control-` 开头的字段：

1. **Access-Control-Allow-Origin**：这个值表示服务器允许请求的 `Origin` 地址，一般服务器会设置为 `*`，表示接受任意域名的请求。
2. **Access-Control-Allow-Credentials**：这个值是一个布尔值，它反应服务器是否允许发送 Cookie。默认情况下，服务器禁止发送 Cookie 来完成 CORS 验证。
3. **Access-Control-Expose-Headers**：这个值表示服务器提供的 CORS 响应字段。默认情况下，浏览器的 XMLHttpRequest 对象只能拿到基本字段的数据，如果浏览器想拿到其他字段，则服务器必须在这里进行指定。

**服务器如果判断到 Origin 指定的源，不在许可范围内，这时，服务器还是会返回一个正常的 HTTP 回应（响应码同样是 200），但就不会携带上述的特殊头信息**。

当浏览器发现这个响应的头信息没有包含 `Access-Control-Allow-Origin` 这些特殊字段时，浏览器就知道这个请求跨域了，需要被禁止。

# OPTIONS 预检请求

CORS 机制除了上述的头信息实现方法外，还可以通过 OPTIONS 预检请求来实现这个机制。

**OPTIONS 预检请求会在浏览器发现请求跨域后，正式发送 HTPP 请求前，进行发送**。

**浏览器会通过 OPTIONS 预检请求询问服务器：当前网页所在的域名是否在服务器的许可名单之中**，以及可以使用哪些 HTTP 动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的 HTTP 请求，否则就报错。

浏览器的 OPTIONS 预检请求同样是全自动发送的，用户不需要进行什么额外的操作。

比如下面的 OPTIONS 预检请求头信息：

```http
OPTIONS /cors HTTP/1.1
Origin: http://api.bob.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

在这段预检头信息中，我们需要关注下面的这三个字段：

1. **Origin**：表示这个请求是从哪里来的。
2. **Access-Control-Request-Method**：表示这个请求会用到的 HTTP 方法，这里是 PUT。
3. **Access-Control-Request-Headers**：表示这个请求会额外发送的头信息字段，这里是 X-Custom-Header。

在服务器收到 OPTIONS 请求以后，服务器便会提前请求头信息，然后对 Origin、Access-Control-Request-Method 和 Access-Control-Request-Headers 这些字段进行检查。当检查结果在允许名单中时，服务器便会确认允许跨源请求，做出响应。

服务器响应的请求头一般如下所示：

```http
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Content-Type: text/html; charset=utf-8
Content-Encoding: gzip
Content-Length: 0
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain
```

在这个响应中，我们可以关注以下的字段：

- **Access-Control-Allow-Origin**：这个用以表示服务支持的域，当设置为 `*` 号时，表示。
- **Access-Control-Allow-Methods**：这个用以表示支持的 HTTP 方法。
- **Access-Control-Allow-Headers**：这个用以表示支持的额外请求头信息。

如果服务器否定了 OPTIONS 请求，服务器还是会返回一个正常的 HTTP 回应，但这个 HTTP 请求是不携带 CORS 相关的头信息字段，比如 Access-Control-Allow-Origin。这时，浏览器因为无法从服务器响应中获取 CORS 头信息，所以会认定服务器不同意预检请求，从而触发一个错误（`Origin xxx is not allowed by Access-Control-Allow-Origin.`），禁止 HTTP 请求发送。
