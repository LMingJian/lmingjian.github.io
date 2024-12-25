---
title: 什么是 Xml
date: 2024-12-25T10:34:34+08:00
author: LiangMingJian
---

# 简介

XML 是纯文本格式，在许多方面类似于 HTML。XML 由 XML 元素组成，每个 XML 元素包括一个开始标记 `<>` ，一个结束标记 `</>` 以及两个标记之间的内容。标记是对文档存储格式和逻辑结构的描述，可以包括注释、引用、字符数据段、起始标记、结束标记、空元素、文档类型声明 ( DTD ) 和序言。

# 编写规则

## 必须有声明语句，作为 XML 文档的第一句

```xml
<?xml version="1.0" encoding="utf-8"?>
```

## 区分大小写

在XML文档中，大小写是有区别的。A 和 a 是不同的标记。因此注意在写元素时，前后标记的大小写要保持一致。

## XML文档有且只有一个根元素

标准格式的 XML 文档有且仅有一个根元素，紧接着声明后面建立，其他元素都是这个根元素的子元素，根元素完全包括文档中其他所有的元素。根元素的起始标记要放在所有其他元素的起始标记之前，根元素的结束标记要放在所有其他元素的结束标记之后。

## 属性值使用引号

在HTML代码里面，属性值可以加引号，也可以不加。但是XML规定，所有属性值必须加引号，否则将被视为错误。

## 所有的标记必须有相应的结束标记

在HTML中，标记可以不成对出现，而在XML中，所有标记必须成对出现，有一个开始标记，就必须有一个结束标记，否则将被视为错误。

## 所有的空标记也必须被关闭

在XML中，规定所有的标记必须有结束标记。

# 示例

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="osg.AndroidExample"
      android:installLocation="preferExternal"
      android:versionCode="1"
      android:versionName="1.0">
    <uses-sdk android:targetSdkVersion="8" android:minSdkVersion="8"></uses-sdk>
    <uses-feature android:glEsVersion="0x00020000"/> 
    <uses-permission android:name="android.permission.INTERNET"/>
    <application android:label="@string/app_name" android:icon="@drawable/osg">
        <activity android:name=".osgViewer" android:label="@string/app_name" android:screenOrientation="landscape"> 
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest>
    <uses-sdk>69</uses-sdk>
    <application>
        <intent-filter>85</intent-filter>
        <activity>88</activity>
    </application>
</manifest>
```
