---
title: 如何使用 Gitee+PicGo 搭建图床
date: 2024-12-25T10:30:31+08:00
author: LiangMingJian
---

# 简介

使用 Gitee 作为博客图床，存储图片。通过 PicGo 进行图床的管理，图片的上传。

（自 2022 年起，Gitee 的图片禁止外链访问，但仍可以通过这个进行图片保存，统一图片名称格式，然后同步到本地访问）

# Gitee 配置

1.新建一个公有仓库
2.配置私人令牌(token)，在`设置`中找到`安全设置`，点击`私人令牌`

![](/_images/drawingbed/img/202204291737792.png)

3.点击`新建`，仅选择下图两项，提交并验证密码后会展示`token`，记录下来，后面操作会使用到。（注意：这个令牌只会明文显示一次）

![](/_images/drawingbed/img/202204291737186.png)

# PicGO 配置

a.下载软件：[ Molunerfinn/PicGo · GitHub ](https://github.com/Molunerfinn/PicGo/releases)，并安装插件：`gitee-uploader`，注意插件的安装需要使用`node.js`
b.选择`图床设置`，选择`gitee`
c.进行配置

- **repo**：用户名/仓库名称（仓库地址后面那一段）
- **branch**：分支，填写master
- **token**：填入前面获取的`私人令牌`
- **path**：路径，一般填写img
- **customPath**：提交消息，可不填
- **customURL**：自定义地址，可不填

d.`上传区`选择`gitee`便可上传图片