---
title: OpenSSL 证书签发
date: 2022-12-05
author: LM
---

## 1.SSL

SSL(Secure Socket Layer)，为 Netscape 所研发，利用数据加密技术来保障数据在 Internet 上的传输安全，确保数据在网络上的传输过程中不会被截取。

SSL 协议位于 TCP/IP 协议与各种应用层协议之间，为数据通讯提供安全支持。SSL 协议可分为两层：

- SSL 记录协议（SSL Record Protocol）：它建立在可靠的传输协议之上，为高层协议提供数据封装、压缩、加密等基本功能的支持。
- SSL 握手协议（SSL Handshake Protocol）：它建立在 SSL 记录协议之上，用于在实际的数据传输开始前，通讯双方进行身份认证、协商加密算法、**交换加密密钥**等。

进行 SSL 握手时，需要交换客户端与服务器彼此的证书。

## 2.生成私钥与证书

```bash
[root@proxy ~]# cd /usr/local/nginx/conf                                 # 进入到目录下生成证书秘钥
[root@proxy conf]# openssl genrsa > cert.key                            # 生成私钥,文件名必须与配置文件内相同
[root@proxy conf]# openssl req -new -x509 -key cert.key > cert.pem     # 生成证书,需要输入信息
Country Name (2 letter code) [XX]: china                              # 国家
State or Province Name (full name) []:hunan                           # 省份
Locality Name (eg, city) [Default City]:changsha                      # 城市
Organization Name (eg, company) [Default Company Ltd]:xxx             # 公司名
Organizational Unit Name (eg, section) []:xxx                         # 单位名
Common Name (eg, your name or your server is hostname) []:主机名       # 主机名 hostname 查看
```

## 3.注意

**请注意密钥文件的编码格式**，在 Windows 中，密钥文件可能被编码为错误格式，需要使用记事本打开密钥文件验证编码。如果显示 UTF-8-BOM，UTF-16 等格式，则需要将其更改为 UTF-8，保存文件后，重试。