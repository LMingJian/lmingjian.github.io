---
title: 如何设置 Linux 的 SSH 端口和开机自启
date: 2024-12-25T13:55:22+08:00
author: LiangMingJian
---

# 需求

SSH 是 Linux 服务器中的一个重要服务，通过 SSH，开发者可以远程连接服务器。

一般情况下，SSH 服务开机自启，不需要开发者进行配置，但某些时候，有些地方可能会要求开发者修改 SSH 的端口号，以提高安全性。

# 修改端口号

**1.定位配置文件并进入编辑模式**

```bash
vi /etc/ssh/sshd_config
```

**2.修改配置**

在 sshd_config 文件中，开发者可以通过修改以下内容实现一些常用的配置：

- **Port 22**：ssh 端口，在修改后需要同步更新防火墙
- **AddressFamily any**：支持 IPv4（inet） 或 IPv6（inet6）
- **ListenAddress 0.0.0.0**：限定访问 IP，0.0.0.0 时支持所有 IP
- **PermitRootLogin no**：禁止 root 直接登录，yes 时同意
- **PasswordAuthentication no**：禁用密码登录，只能使用密钥登录
- **MaxAuthTries 3**：最大认证尝试次数

![](_images/drawingbed/img/Pasted%20image%2020251016105130.png)

**3.重启服务生效配置**

ssh 服务在 Linux 中的名称是 sshd，全称 ssh daemon，ssh 守护进程。

```bash
systemctl restart sshd
```

# 开机自启

```bash
systemctl start sshd
systemctl enable sshd
```
