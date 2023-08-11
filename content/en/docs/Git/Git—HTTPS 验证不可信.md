---
title: HTTPS 验证不可信
date: 2023-08-11
author: LM
---

## 1.为什么会出现验证不可信

在使用 Github 时，由于中国的网络环境问题，往往需要使用到某些魔法工具，此时 git 在拉取或提交时就会出现如 `git SSL certificate problem: unable to get local issuer certificate`的错误，git 不能保证经过这些魔法工具的数据是可信的。

## 2.关闭 ssl 验证以避免报错

```
git config --global http.sslVerify false
```

