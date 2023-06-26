---
title: Nginx 的配置
date: 2020-03-02
author: LM
---

## 1.Nginx的配置文件

Nginx 的配置分为全局，Event，Http，Server 和 Location。

![](/images/drawingbed/img/202205051055087.png)

### 1.全局块

该部分配置 Nginx 全局内容，包括下面几个部分：

- 配置运行 Nginx 服务器用户（组）
- worker process 数
- Nginx 进程 PID 存放路径
- 错误日志的存放路径
- 外部配置文件的引入

```nginx
user user [group];
user nobody nobody; 
# 指定可以运行 Nginx 服务器的用户，如果 user 指令不配置或者配置为 nobody nobody，则默认所有用户都可以启动 Nginx 进程
-------------------------------------------------------------------------------
worker_processes number;
# number：Nginx 进程最多可以产生的 worker process 数，配置为 auto：Nginx 进程将自动检测
-------------------------------------------------------------------------------
error_log file;
# file：日志输出到某个文件 file，配置为 stderr 日志输出到标准错误输出
error_log logs/error.log; # 将日志输出到 logs/error.log
--------------------------------------------------------------------------------
pid file;
# file：指定存放路径和文件名称，不指定默认置于路径 logs/nginx.pid
----更多配置---------------------------------------------------------------------
include file;
# 该指令主要用于将其他的 Nginx 配置或者第三方模块的配置引用到当前的主配置文件中
accept_mutex on;
# 该指令默认为 on 状态，表示会对多个 Nginx 进程接收连接进行序列化，防止多个进程对连接的争抢
multi_accept off;
# 该指令默认为 off 状态, 意指每个 worker process 一次只能接收一个新到达的网络连接。若想让每个 Nginx 的 worker process 都有能力同时接收多个网络连接, 则需要开启此配置
```

### 2.Events 块

该部分配置 Nginx 服务器与用户的网络连接事件，包括：

- 设置网络连接的序列化
- 是否允许同时接收多个网络连接
- 事件驱动模型的选择
- 最大连接数的配置

```nginx
use model;
# 事件驱动模型的选择，可选择项包括：select、poll、kqueue、epoll、rtsig
----------------------------------------------------------------------------------
worker_connections number;
# 最大连接数的配置, number 默认值为 512, 表示允许每一个 worker process 可以同时开启的最大连接数
```

### 3.Http 块

这部分是负责网页相关的配置

- 定义 MIMI-Type
- 自定义服务日志
- 允许 sendfile 方式传输文件
- 连接超时时间
- 单连接请求数上限

```nginx
include mime.types;
default_type mime-type;
# 定义 MIME-Type 网络资源的媒体类型，也即前端请求的资源类型
# mime.types 是一个 types 结构文件，里面包含了各种浏览器能够识别的 MIME 类型以及对应类型的文件后缀名字
----------------------------------------------------------------------------------------
access_log path [format];
# path：自定义服务日志的路径 + 名称
# format：可选项，自定义服务日志的字符串格式。其也可以使用 log_format 定义的格式
----------------------------------------------------------------------------------------
sendfile on;
sendfile_max_chunk size;
# 前者用于开启或关闭使用 sendfile() 传输文件，默认 off
# 后者指令若 size>0，则Nginx进程的每个 worker process 每次调用 sendfile() 传输的数据了最大不能超出此值；若 size=0 则表示不限制。默认值为 0
---------------------------------------------------------------------------------------
keepalive_timeout timeout [header_timeout];
# 连接超时时间配置，timeout 表示 server 端对连接的保持时间，默认75秒
# header_timeout 为可选项，表示在应答报文头部的 Keep-Alive 域设置超时时间："Keep-Alive : timeout = header_timeout"
-----更多配置---------------------------------------------------------------------------
keepalive_requests number;
# 单连接请求数上限, 用于限制用户通过某一个连接向Nginx服务器发起请求的次数
```

### 4.Server 块

这部分是负责服务器相关的配置

- 配置网络监听
- 基于名称的虚拟主机配置
- 基于IP的虚拟主机配置

```nginx
listen IP[:PORT];
listen PORT;                  # 配置监听的IP地址 或 配置监听的端口
listen 192.168.31.177:8080;   # 监听具体IP和具体端口上的连接
listen 192.168.31.177;        # 监听IP上所有端口上的连接
listen 8080;                  # 监听具体端口上的所有IP的连接
```

### 5.Location 块

本地文件配置

-  请求根目录配置
-  更改 location 的 URI
-  网站默认首页配置

```nginx
location [ = | ~ | ~* | ^~ ] uri {...};
# 这里的 uri 分为标准 uri 和正则 uri
# "="：用于标准 uri 前，要求请求字符串与 uri 严格匹配，一旦匹配成功则停止
# "~"：用于正则 uri 前，并且区分大小写
# "~*"：用于正则 uri 前，但不区分大小写
# "^~"：用于标准 uri 前，要求 Nginx 找到标识 uri 和请求字符串匹配度最高的 location 后，立即使用此 location 处理请求，而不再使用 location 块中的正则 uri 和请求字符串做匹配
location = /404.html{...};  # 示例
-------------------------------------------------------------------------------------------------
root path;
# path：Nginx 接收到请求以后查找资源的根目录路径
-------------------------------------------------------------------------------------------------
index file;
# 设置网站的默认首页
# file:可以包含多个用空格隔开的文件名，首先找到哪个页面，就使用哪个页面响应请求
```

## 2.Nginx 配置 SSL 及 HTTP 跳转到 HTTPS 示例

```nginx
# Nginx 配置 SSL 并把 Http 跳转到 Https，需修改 Nginx.conf 配置文件如下
server {

  listen 80;
  server_name www.example.com;
  return 301 https://www.example.com$request_uri;

  # 把 http 重定向到 https 使用了nginx 的重定向命令
  # return 301 https://www.example.com$request_uri;
}

server {

  listen 443;
  server_name www.example.com;
  root /data/release/weapp/uploadFiles;

  # 开启ssl功能
  ssl on;

  # 配置ssl证书，直接用.pem和.key文件的绝对路径
  ssl_certificate/data/release/nginx/ssl_file.pem;
  ssl_certificate_key/data/release/nginx/ssl_file.key;
  ssl_session_timeout 5m;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers ECDHE - RSA - AES128 - GCM - SHA256: ECDHE: ECDH: AES: HIGH: !NULL: !aNULL: !MD5: !ADH: !RC4;
  ssl_prefer_server_ciphers on;

  location / {

     proxy_pass http://app_weapp;
     proxy_http_version 1.1;
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header Connection 'upgrade';
     proxy_set_header Host $host;
     proxy_cache_bypass $http_upgrade;

  }

  location /images/ {
    autoindex on;
  }

  # 配置 uri， ~ 用于正则 uri 前，其中 .(png|jpg) 为正则表达式
  # root 用于配置接收到请求以后查找资源的根目录路径

  location ~ \.(png|jpg) {
     root /data/release/weapp/uploadFiles;
  }

  error_page 404 /404.html;

  location = /40x.html {
  }

  error_page 500 502 503 504 /50x.html;

  location = /50x.html {
  }
}
```

