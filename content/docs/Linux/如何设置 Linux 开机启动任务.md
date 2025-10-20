---
title: 如何设置 Linux 开机启动任务
date: 2024-12-25T13:55:27+08:00
author: LiangMingJian
---

# 需求

在 Linux 运维时，往往编写了一些 Shell 脚本，这些脚本中，有些我们希望在 Linux 系统开机后自动运行，实现自动维护，此时如何实现开机自启就成了问题。

# 使用 /etc/rc.d/rc.local 文件

**这是最简单粗暴的方法**。

`/etc/rc.d/rc.local` 是 Linux 系统开机启动任务文件 `/etc/rc.local` 的软链接，其作用是在系统启动的最后阶段执行用户自定义命令或脚本。

**特别注意，`/etc/rc.d/rc.local` 要生效，则必须在配置后需要手动添加可执行权限，让脚本允许被运行**。

```bash
vi /etc/rc.d/rc.local
chmod +x /etc/rc.d/rc.local
```

![](_images/drawingbed/img/Pasted%20image%2020251017092139.png)

# 使用 systemd 服务

通过将目标脚本创建为系统服务，实现更精细化的开机启动控制。

**1.创建服务文件**

在系统目录 `/etc/systemd/system/` 中创建一个 `start.service` 的服务。

```bash
cd /etc/systemd/system/
vi mystart.service
```

在 `start.service` 服务文件中添加以下内容。

```bash
[Unit]
# 服务描述
Description=MyStartService
# 依赖网络服务
After=network.target

[Service]
# 启动命令
ExecStart=/root/your_start_script.sh
# 只在失败时重启
Restart=on-failure
# 重启间隔
RestartSec=5s
# 允许重启次数
StartLimitBurst=10
# 执行用户
User=root

[Install]
# 多用户模式下启用
WantedBy=multi-user.target
```

> 特别注意，执行脚本中必须添加 `#!/bin/bash` 头，以避免脚本无法识别。同时脚本中所有涉及路径的内容，应当使用绝对路径，如 `/root/data.txt`，以避免输出丢失。

**2.启动服务并设置为开机自启**

```bash
systemctl start mystart.service
systemctl status mystart.service
systemctl enable mystart.service 
```

**3.删除服务**

```bash
# 停止并关闭自动启动
systemctl stop mystart.service
systemctl disable mystart.service

# 删除对应的服务文件
rm /etc/systemd/system/mystart.service

# 检查服务是否还存在
systemctl status mystart.service
systemctl list-unit-files | grep mystart
```

# 使用 crontab 的 @reboot 功能

crontab 是 Linux 系统中的定时任务系统，用户可以通过编辑该系统文件来实现定时任务的生效，开机自启任务的生效。

**1.进入 cronta 系统的编辑模式**

```bash
crontab -e
```

**2.在文件中加入开机自启任务**

```bash
@reboot /root/your_script.sh
```

> 特别注意，所使用的脚本必须使用绝对路径。
