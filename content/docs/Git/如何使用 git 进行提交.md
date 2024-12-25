---
title: 如何使用 git 进行提交
date: 2024-12-25T13:47:11+08:00
author: LiangMingJian
---

#  配置用户名与邮箱

在所有提交前，都需要配置提交的用户名和邮箱。

```bash
git config --global user.name "???"
git config --global user.email "????"
# --global 去除则针对单个项目
```

# 克隆仓库然后提交

```bash
git clone ssh://url/somethingtest
cd somethingtest
touch README.md
git add -A
git commit -m "new"
git push -u origin master
```

# 使用已有文件创建仓库

```bash
cd existing_folder
git init
git remote add origin ssh://url
git add -A
git commit -m "Initial commit"
git push -u origin master
```

# 日常上传文件

```bash
git add -A
git commit -m "Update"
git push -u origin master
```

# Ex.拉取文件

```bash
git pull origin master
```

# Ex.强制推送

```bash
git push -f origin master
```
