---
title: Flutter 默认下载环境配置
date: 2021-04-25
author: LM
---

## 1.环境设置

由于 Flutter 默认配置的下载服务器在国外，往往会出现网络波动无法下载的情况，因此需要重新配置国内镜像，避免出现无法下载情况。在终端命令或 Windows 环境变量中增加键值。

```bash
$env:PUB_HOSTED_URL="https://pub.flutter-io.cn"
$env:FLUTTER_STORAGE_BASE_URL="https://storage.flutter-io.cn"
```

以上方式只是临时使用，如果需要长期生效，请在对应系统的环境管理中填写相应变量。

{{< details "参考文件" >}}  
1：[ 在中国网络环境下使用 Flutter - @Flutter 中文开发者网站 ](https://flutter.cn/community/china)
{{< /details >}}

