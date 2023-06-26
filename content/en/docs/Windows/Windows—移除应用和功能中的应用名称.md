---
title: Windows 系统移除应用和功能中的应用名称
date: 2022-12-06
author: LM
---

## Question

Windows 设置里的【应用和功能】模块有时候会存在异常的应用名称（无法通过应用和功能卸载的应用名称），此时需要手动删除该内容。

## Method

通过修改注册表中数据进行移除。

## Step

1. `Win + R` 打开运行
2. 输入 `regedit` 打开注册表编辑器
3. 定位：`计算机\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall`
4. 修改或删除目标内容

