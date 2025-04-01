---
title: 修复 Remote Ripple 缺少 .Net 框架的问题
date: 2025-03-01T11:29:34+08:00
author: LiangMingJian
---

# BUG 描述

局域网内远程工具 Remote Ripple 在启动时缺少了 .Net 框架。

# Resolution

> [Kvarkas](https://github.com/Kvarkas) [on May 23, 2022](https://github.com/mRemoteNG/mRemoteNG/issues/2229#issuecomment-1134226562)
> 
> hello, we are using .net 6, from [there](https://dotnet.microsoft.com/en-us/download/dotnet/6.0) you need download [.NET Desktop Runtime 6.0.5](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-desktop-6.0.5-windows-x64-installer)

根据上述 Issues 中的描述，Remote Ripple 使用的的 .Net 版本是 .net 6，需要下载安装。

注意，高版本与低版本的 .net 似乎不兼容，要想 Remote Ripple 正常使用，必须下载 .net 6 而不能是更高版本。
