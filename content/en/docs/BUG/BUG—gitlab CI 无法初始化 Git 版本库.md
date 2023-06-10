---
title: gitlab CI 无法初始化 Git 版本库
date: 2021-01-05
author: LMingJian
---

## BUG 描述

gitlab CI 出现报错。

```bash
fatal: git fetch-pack: expected shallow list
fatal: The remote end hung up unexpectedly
```

## Resolution

这是由于 git 版本过老不支持新的 API，需要升级 git。

```bash
# 安装源
yum install http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-2.noarch.rpm
# 安装 git
yum install git
# 更新 git
yum update git
```
