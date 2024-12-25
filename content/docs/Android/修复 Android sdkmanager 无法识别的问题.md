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

# Resolution

在下载 cmdline tools 压缩包后，需要将解压缩后的目录从 cmdline tools 重命名为 tools，并将其放在目录 `.../Android/cmdline tools`下。

{{< details "参考文件" >}}
1：[ cmdline-tools: could not determine SDK root  @stackoverflow ](https://stackoverflow.com/questions/65262340/cmdline-tools-could-not-determine-sdk-root)
{{< /details >}}
