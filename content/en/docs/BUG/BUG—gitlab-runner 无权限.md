---
title: gitlab-runner 无权限
date: 2021-01-06
author: LMingJian
---

## BUG 描述

gitlab-runner 在服务器运行时提示没有权限。

## Resolution

需要手动将 gitlab-runner 服务设置为 root 用户。

```shell
ps aux|grep gitlab-runner  # 查看当前 runner 用户

sudo gitlab-runner uninstall  # 删除 gitlab-runner

gitlab-runner install --working-directory /home/gitlab-runner --user root  # 设置 root 用户

sudo service gitlab-runner restart  # 重启gitlab-runner

ps aux|grep gitlab-runner # 再次执行会发现 --user 的用户名已经更换成root了
```

