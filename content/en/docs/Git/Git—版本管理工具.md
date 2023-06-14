---
title: Git 版本管理工具
date: 2021-01-05
author: LM
---

## 1.Git 搭载

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

Windows 通过在 [ Git官网 ](https://git-scm.com/) 下载安装来搭载 Git 环境，Linux 通过对应的包管理工具进行下载安装。

需要注意的是，Git 能跟踪所有的文件，但其无法追踪一个**空文件夹**，当用户需要追踪一个空文件夹的时候，可以将一个称为 **.gitkeep** 的文件放在文件夹里。

##  2.配置用户名与邮箱

```bash
git config --global user.name "???"
git config --global user.email "????"
# --global 去除则针对单个项目
```

## 3.克隆仓库

```bash
git clone ssh://url
cd somethingtest
touch README.md
git add -A
git commit -m "new"
git push -u origin master
```

## 4.上传文件创建仓库

```bash
cd existing_folder
git init
git remote add origin ssh://url
git add -A //.
git commit -m "Initial commit"
git push -u origin master
```

## 5.上传文件

```bash
git add -A
git commit -m "Update"
git push -u origin master
```

## 6.拉取文件

```bash
git pull origin master
```

## 7.强制推送

```bash
git push -f origin master
```

