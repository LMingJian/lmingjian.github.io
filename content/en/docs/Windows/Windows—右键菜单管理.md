---
title: Windows 系统的右键菜单管理
date: 2023-03-17
author: LM
---

## Question

如何对 Windows 右键菜单的内容进行管理（修改名称，删除内容）。

## Method

修改注册表。

## Step

1. `Win + R` 打开运行
2. 输入 `regedit` 打开注册表编辑器
3. 定位：`计算机\HKEY_CLASSES_ROOT\Directory\Background\shell`
4. 修改或删除目标内容

