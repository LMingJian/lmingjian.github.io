---
title: 如何修改 VSCode 底部状态栏的颜色
date: 2024-12-25T16:14:37+08:00
author: LiangMingJian
---

# 实现

在 VSCode 设置中搜索`workbench.colorCustomizations`，然后点击在`setting.json`中进行编辑修改。

```json
{
    "workbench.colorCustomizations": {
        "statusBar.background" : "#008cff",
        "statusBar.noFolderBackground" : "#008cff",
        "statusBar.debuggingBackground": "#008cff",
    },
}
```
