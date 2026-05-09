---
title: Bat 脚本：使用 ffmpeg 批量缩小文件夹内图片
date: 2026-02-05T14:21:44+08:00
author: LiangMingJian
---

# 需求

使用 ffmpeg 将当前文件夹内的所有图片缩小，并保持原名字，减少空间占用。

# 代码

```shell
@echo off
for %%a in (*.jpg) do (
    ffmpeg -i "%%a" -q:v 5 "temp_%%~na.jpg" -y
    del "%%a"
    ren "temp_%%~na.jpg" "%%a"
)
```
