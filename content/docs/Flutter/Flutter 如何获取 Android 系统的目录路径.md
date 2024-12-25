---
title: Flutter 如何获取 Android 系统的目录路径
date: 2024-12-25T13:46:01+08:00
author: LiangMingJian
---

# 添加依赖

在项目的 `pubspec.yaml` 文件中添加依赖，使用`path_provider`实现系统路径的读取。

```yaml
dependencies:
     path_provider: ^1.6.14
```

执行`flutter pub get`命令拉取文件。

# 获取文件路径

- `getTemporaryDirectory`：临时目录，适用于下载的缓存文件，此目录随时可以清除，此目录为应用程序私有目录，其他应用程序无法访问此目录。
- `getApplicationSupportDirectory`：应用程序可以在其中放置应用程序支持文件的目录的路径。
- `getLibraryDirectory`：应用程序可以在其中存储持久性文件，备份文件以及对用户不可见的文件的目录路径，例如 storage.sqlite.db。
- `getApplicationDocumentsDirectory`：应用程序可能在其中放置用户生成的数据或应用程序无法重新创建的数据的目录路径。
- `getExternalStorageDirectory`：应用程序可以访问顶级存储的目录的路径。由于此功能仅在Android上可用，因此应在发出此函数调用之前确定当前操作系统。
- `getExternalCacheDirectories`：存储特定于应用程序的外部缓存数据的目录的路径。 这些路径通常位于外部存储（如单独的分区或SD卡）上。 电话可能具有多个可用的存储目录。
- `getExternalStorageDirectories`：可以存储应用程序特定数据的目录的路径。 这些路径通常位于外部存储（如单独的分区或SD卡）上。
- `getDownloadsDirectory`：存储下载文件的目录的路径，这通常仅与台式机操作系统有关。

# Ex.Android 文件存储

Android 文件存储分为内部存储和外部存储。

## 内部存储

用于保存应用的私有文件，其他应用无法访问这些数据，没有 root 权限的手机无法在手机的文件管理应用中看到此目录，包名下具体的目录结构为：

- `cache`：对应 getTemporaryDirectory 方法，用于缓存文件，此目录随时可能被系统清除。
- `files`：对应 getApplicationSupportDirectory 方法。
- `code_cache`：此目录存储 Flutter 相关代码和资源。
- `shared_prefs`： SharePreferences 的默认路径。
- `app_flutter`：对应 getApplicationDocumentsDirectory 方法。
- `app_flutter/dbName`：使用 sqlite 的默认路径，sqlite 也可以指定位置。

内部存储的特点：

- 安全性，其他应用无法访问这些数据。
- 当应用卸载的时候，这些数据也会被删除，避免垃圾文件。
- 不需要申请额外权限。
- 存储的空间有限，此目录数据随时可能被系统清除，也可以通过设置中的清除数据清除。

## 外部存储

外部存储可以通过手机的文件管理应用查看，不过这里存在一个特殊的目录：Android/data/package，和内部存储目录非常相似，一个包名代表一个应用程序，其目录结构为：

- `cache`：缓存目录，对应 getExternalCacheDirectories 方法。
- `files`：对应 getExternalStorageDirectories 方法。

此目录的特点：

- 当应用卸载的时候，这些数据也会被删除，避免垃圾文件。
- 不需要申请额外权限。
- 空间大且不会被系统清除，可以通过设置中的清除数据可以清除此目录。
- 用户可以直接对文件进行删除、导入操作。

{{< details "参考文件" >}} 
1： [【Flutter 实战】文件系统目录  @老孟程序员 ](https://www.jianshu.com/p/2eafae001f55)
{{< /details >}}
