---
title: 如何设置 Linux 开机启动任务
date: 2024-12-25T13:55:27+08:00
author: LiangMingJian
---

# Linux 系统文件启动顺序

系统启动时按顺序加载以下的配置文件，要设置开机启动任务即修改以下部分文件。

```bash
/etc/profile
/root/.bash_profile
/etc/bashrc
/root/.bashrc
/etc/profile.d/*.sh
/etc/profile.d/lang.sh
/etc/sysconfig/i18n
/etc/rc.local (/etc/rc.d/rc.local)
```

# 修改 /etc/rc.local 或 /etc/rc.d/rc.local

```bash
# 1.编辑 rc.local 文件
vi /etc/rc.local
 
# 2.修改 rc.local 文件，在 exit 0 前面加入需要执行命令。保存并退出。
/etc/init.d/mysqld start        # mysql 开机启动
/etc/init.d/nginx start         # nginx 开机启动
/bin/bash /test.sh >/dev/null 2>/dev/null  # test 脚本开机执行
 
# 3.最后修改 rc.local 文件的执行权限
chmod +x /etc/rc.local
chmod 755 /etc/rc.local
```

# 将脚本放到 /etc/profile.d/ 下

Linux 系统启动后，会检查 `/etc/profile.d/`  目录下所有脚本，并依次自动执行。

# 通过 chkconfig 命令设置

```bash
# 1.将(脚本)启动文件移动到 /etc/init.d/ 或者 /etc/rc.d/init.d/ 目录下。（前者是后者的软连接）
# test.sh 文件前 3 行必须写入以下内容，分别告知系统使用的命令，运行级别，描述
# 【#!/bin/sh】【#chkconfig: 35 20 80】【#description: http server】
mv /www/wwwroot/test.sh /etc/rc.d/init.d
 
# 3.增加脚本的可执行权限
chmod +x /etc/rc.d/init.d/test.sh
 
# 4.添加脚本到开机自动启动项目中。添加到chkconfig，开机自启动。
cd /etc/rc.d/init.d
chkconfig --add test.sh
chkconfig test.sh on
 
# 5.关闭开机启动 
chkconfig test.sh off
 
# 6.从 chkconfig 管理中删除 test.sh
chkconfig --del test.sh
 
# 7.查看chkconfig管理
chkconfig --list test.sh
```

{{< details "参考文件" >}} 
1：[ Linux 添加开机启动方法(服务/脚本)  @jb51 ](https://www.jb51.net/article/176257.htm)
{{< /details >}}
