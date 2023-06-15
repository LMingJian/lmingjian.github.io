---
title: VSCode 修改底部状态颜色
date: 2021-08-27
author: LM
---

## 1.改变颜色

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

