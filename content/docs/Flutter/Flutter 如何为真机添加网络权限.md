---
title: Flutter 如何为真机添加网络权限
date: 2024-12-25T13:46:35+08:00
author: LiangMingJian
---

# 网络权限

在 AndroidManifest.xml 文件中添加下列代码。

```xml
// android/app/src/main/AndroidManifest.xml
<manifest >
    <application>
    ............
    </application>
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
</manifest>
```
