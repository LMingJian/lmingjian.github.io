---
title: Flutter 启动页的添加
date: 2022-08-22
author: LM
---

## 1.前言

由于 Flutter 默认没有启动图，而 App 启动到 Flutter 第一帧渲染结束前需要一定时间加载运行库，所以打开 App 会先显示难看的白屏。

## 2.Android设置

Android 提供了启动页的概念，用于在应用初始化的过程中展示一个`Drawable`。

### 1.准备图片

默认名称为`launch_image.png`，打开目录`android/app/src/main/res/`，按照支持的不同设备分辨率，放在各个 mipmap 目录下。

### 2.修改配置文件

编辑`android/app/src/main/res/drawable/launch_background.xml`，将`<!-- You can insert your own image assets here -->`下的`<item>`标签里面的内容反注释，就会将`launch_image.png`居中显示在启动页上，例如：

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- Modify this file to customize your launch splash screen -->
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@android:color/white" />

    <!-- You can insert your own image assets here -->
    <item>
        <bitmap
            android:gravity="center"
            android:src="@mipmap/launch_image" />
    </item>
</layer-list>
```

{{< details "参考文件" >}} 
1：[ Flutter设置启动页（Splash Screen）@agang_19 ](https://www.cnblogs.com/agang-php/p/16482023.html)
{{< /details >}}