---
title: 如何修改 Windows 的右键菜单项
date: 2024-12-25T16:17:22+08:00
author: LiangMingJian
---

# 问题

在 Windows 日常使用中，由于卸载异常，Windows 的右键菜单可能会出现一些错误的内容。或者在日常使用中，用户需要定制化的修改 Windows 的右键菜单。

此时，可以通过修改注册表来进行移除或修改。

# 解决

1.输入`Win + R` 打开运行。

2.输入 `regedit` 打开注册表编辑器。

3.定位到`计算机\HKEY_CLASSES_ROOT\Directory\Background\shell`。

4.修改或删除目标内容
