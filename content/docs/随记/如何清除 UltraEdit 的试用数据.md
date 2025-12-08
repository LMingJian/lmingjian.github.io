---
title: 如何清除 UltraEdit 的试用数据
date: 2024-12-25T10:17:31+08:00
author: LiangMingJian
---

# 软件版本：28.20.0.92

UltraEdit 是一个文本查看工具，支持查看 16 进制文件。

UltraEdit 是一个商业工具，有 30 天免费使用，但可以通过修改注册表键值删除试用数据。

# 清理注册表键值

```
HKCU\SOFTWARE\Classes\WOW6432Node\CLSID\{9b4c79e8-d476-48e1-ad17-2253d0531ebb}
HKCU\SOFTWARE\Classes\WOW6432Node\CLSID\{bf2611c5-cf99-4e19-be15-83e593688709}
HKCU\SOFTWARE\Classes\WOW6432Node\CLSID\{c0bf323d-faa8-4b16-bdc9-92c6acb76dc1}
```

# 清理存储文件

```
%userprofile%\AppData\Roaming\IDMComp\UltraEdit\license\uedit32_v.spl
%ProgramData%\IDMComp\UltraEdit\license\uedit32.spl
```
