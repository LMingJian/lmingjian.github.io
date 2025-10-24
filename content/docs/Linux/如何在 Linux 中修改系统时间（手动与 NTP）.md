---
title: 如何在 Linux 中修改系统时间（手动与 NTP）
date: 2024-12-25T13:55:57+08:00
author: LiangMingJian
---

# 通过 date 手动修改时间

在 Linux 中，修改系统时间可以通过 date 命令来进行实现。

**查看时间和日期**

```bash
date
# Wed Oct 22 10:40:28 CST 2025

date -R
# Wed, 22 Oct 2025 10:40:45 +0800
# 携带时区信息，+0800，东8区
```

**修改时间**

```bash
# 只修改年月日
date -s "2025-10-22"

# 只修改时分秒
date -s "15:30:00"

# 全部修改
date -s "2025-10-22 15:30:00"
```

**同步硬件时间**

特别的，以上修改都是临时的，如果要保证设备重启后时间还是修改后的内容，需要将时间写入 BIOS。

```bash
hwclock -w
```

# 修改时区

时区的修改需要将 `/usr/share/zoneinfo` 里面的时区文件连接到当前使用的时区文件 `/etc/localtime`。

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

# 通过 ntp 修改时间

NTP 服务的使用需要先安装 NTP 软件包。

**安装 ntp 软件**

```bash
yum install -y ntp
```

安装成功后，便可以通过编辑配置文件 `/etc/ntp.conf` 来设置 NTP 服务地址。

**设置 NTP 服务器**

```bash
vi /etc/ntp.conf
```

![](_images/drawingbed/img/Pasted%20image%2020251022105905.png)

建议修改为：

```bash
# 阿里云服务，prefer 优先使用
server ntp.aliyun.com prefer

# NTP Pool Project，iburst 系统启动后快速同步
server 0.cn.pool.ntp.org iburst
server 1.cn.pool.ntp.org iburst
server 2.cn.pool.ntp.org iburst
server 3.cn.pool.ntp.org iburst
```

**启动 NTP 服务**

```bash
systemctl start ntpd
systemctl enable ntpd
```

特别注意，在启动后需要开放 NTP 端口 123，避免防火墙阻碍。

```bash
firewall-cmd --zone=public --add-port=123/tcp --permanent
firewall-cmd --zone=public --add-port=123/udp --permanent
firewall-cmd --reload
```

**状态检查**

```bash
# 检查服务状态
systemctl status ntpd

# 检查同步状态
ntpq -p

# 检查同步信息
ntpstat
```

![](_images/drawingbed/img/Pasted%20image%2020251022111313.png)
