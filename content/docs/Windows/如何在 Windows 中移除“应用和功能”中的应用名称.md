---
title: 如何在 Windows 中移除“应用和功能”中的应用名称
date: 2024-12-25T16:17:31+08:00
author: LiangMingJian
---

# 问题

在 Windows 设置的”应用和功能“中，有时候由于异常的安装或卸载，导致存在一些无法从应用列表中移除的应用项。

此时，可以通过修改注册表来进行移除。

# 解决

1.按下 `Win + R` 打开运行。

2.输入 `regedit` 打开注册表编辑器。

3.在注册表中定位到：

```
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node
\Microsoft\Windows\CurrentVersion\Uninstall
```

4.修改或删除对应的目标内容。
