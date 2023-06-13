---
title: Flutter 默认下载环境配置
date: 2021-04-25
author: LM
---

## 1.环境设置

由于 Flutter 默认配置的下载服务器在国外，往往会出现网络波动无法下载的情况，因此需要重新配置国内镜像，避免出现无法下载情况。在终端命令或 Windows 环境变量中增加键值。

```bash
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```