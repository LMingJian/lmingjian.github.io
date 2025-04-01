---
title: 如何关闭 Chrome 的系统升级横幅
date: 2025-03-01T11:35:50+08:00
author: LiangMingJian
---

# 需求

用户如果在 Windows7 系统上安装 Google Chrome，在打开浏览器时，浏览器头部会弹出系统升级提示：**想要获得后续谷歌 Chrome 更新，你需要升级到 Win10 或者更高版本。当前设备运行的是 Win7 系统**。

用户如果需要消除该系统提示，可以采用修改注册表的方式进行。

# 实现

在桌面新建文本文档，重命名为**closeChromeUpdateWarning.reg**，添加以下内容后，双击执行。

```text
Windows Registry Editor Version 5.00 

[HKEY_CURRENT_USER\Software\Policies\Google\Chrome] 
"SuppressUnsupportedOSWarning"=dword:00000001
```
