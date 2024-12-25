---
title: 如何忽略 git 提交时的 HTTPS 不可信报错
date: 2024-12-25T13:47:00+08:00
author: LiangMingJian
---

# 为什么会出现验证不可信

在使用 Github 时，由于中国的网络环境问题，往往需要使用到某些魔法工具，此时 git 在拉取或提交时就会出现如 `git SSL certificate problem: unable to get local issuer certificate`的错误，git 不能保证经过这些魔法工具的数据是可信的。

# 关闭 ssl 验证以避免报错

```
git config --global http.sslVerify false
```
