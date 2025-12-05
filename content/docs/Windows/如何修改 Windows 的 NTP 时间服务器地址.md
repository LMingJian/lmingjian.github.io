---
title: 如何修改 Windows 的 NTP 时间服务器地址
date: 2025-12-01T15:29:56+08:00
author: LiangMingJian
---

# 需求

windows 默认使用 `time.windows.com`作为 NTP 时间服务器来同步时间，但在内网环境中，该地址可能无法使用，因此需要手动修改该地址来将其指向内网所提高的 NTP 时间服务器。

# 处理

**1.通过 `Win + S` 搜索并打开控制面板，或在设置中搜索控制面板并打开。**

**2.在控制面板中找到时钟和区域点击进入。**

![](_images/drawingbed/img/Pasted%20image%2020251201153104.png)

**3.在时钟和区域中找到日期和时间点击进入。**

![](_images/drawingbed/img/Pasted%20image%2020251201153115.png)

**4.在日期和时间中点击 Internet 时间栏目，并点击更改设置。**

![](_images/drawingbed/img/Pasted%20image%2020251201153129.png)

**5.最后在弹窗中，勾选与 Internet 时间服务器同步，并修改服务器地址，确定即可。**

![](_images/drawingbed/img/Pasted%20image%2020251201153202.png)
