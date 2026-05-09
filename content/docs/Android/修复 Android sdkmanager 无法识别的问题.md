---
title: 修复 Android sdkmanager 无法识别的问题
date: 2024-12-25T11:00:51+08:00
author: LiangMingJian
---

# BUG 描述

执行 Android cmd 工具 sdkmanager 命令时无法找到对应路径。

```bash
Error: Could not determine SDK root. 
Error: Either specify it explicitly with --sdk_root= or move this package into its expected location: \cmdline-tools\latest\
```

# 原因与解决

上述问题是因为 cmd 工具在单独下载时，生成的目录结构和随 Android SDK 一起下载时不同导致的。

cmd 工具在单独下载解压时，其目录是 `.../cmdline-tools`。但随 Android SDK 一起下载时，它的目录应该是 `.../Android/cmdline-tools/tools`。这就导致了 cmd 工具里面的命令无法被系统找到。

因此，只要将解压缩后的目录从 `cmdline-tools` 重命名为 `tools`，然后再将其移动到目录 `.../Android/cmdline-tools` 下即可，保证最终目录为 `.../Android/cmdline-tools/tools`。

————————————

> 参考文件：[ cmdline-tools: could not determine SDK root  @stackoverflow ](https://stackoverflow.com/questions/65262340/cmdline-tools-could-not-determine-sdk-root)
