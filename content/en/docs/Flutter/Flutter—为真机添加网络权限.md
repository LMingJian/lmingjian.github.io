---
title: Flutter 真机网络权限配置
date: 2021-05-14
author: LM
---

## 1.网络权限

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

