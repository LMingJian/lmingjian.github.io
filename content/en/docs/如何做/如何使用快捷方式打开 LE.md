---
title: 如何使用快捷方式打开 LE
date: 2022-08-11
author: LM
---

## 1.需求

使用快捷方式启动 LE，并完成对应游戏的转区。

## 2.实现

首先进入 LE 安装文件夹，然后打开文件 LEconfig.xml，找到下图中的 Guid 值。

![](/images/drawingbed/img/202308111503546.png)

创建快捷方式，并填入以下命令。

```
"E:\Locale Emulator\LEProc.exe" -runas "773b8f0e-4016-402d-97f9-631c748bf095" "D:\galgame\SATOR\game.exe"
```

