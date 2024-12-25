---
title: 如何在 Linux 中修改系统时间
date: 2024-12-25T13:55:57+08:00
author: LiangMingJian
---

# 查看时区

要查看当前生效的时区，可以通过`date`命令来进行。

```bash
date -R
# Tue, 17 Jan 2017 21:36:23 +0800
# +0800，即东8区
```

# 修改时区

执行：` ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`。通过将系统时区文件指向目标时区文件，来设置时区。

# 查看和修改时间

- `date`：查看时间和日期
- `date -s 11/03/2009`：将系统日期设定成2009年11月3日
- `date -s 17:55:55`：将系统时间设定成下午5点55分55秒
- `hwclock -w`：将当前时间和日期写入BIOS，避免重启后失效
