---
title: 龙头 RPGMaker 方向键异常
date: 2023-08-11
author: LMingJian
---

## BUG 描述

在游玩龙头 RPGMaker 制作的游戏时，无法使用方向键进行控制，启动游戏后会自动往左上跑，一直输出向上和向左信号。

## Resolution

初步怀疑是由于向日葵等远程软件的虚拟设备与 RPGMaker 存在冲突，导致出现上述故障，解决办法为卸载向日葵等远程软件。

检查 Windows 设备管理器，在人机接口设备中查看是否存在 HID virtual device kit standard 等设备，寻找安装该设备的程序进行卸载。

{{< details "参考文件" >}} 
1：[ 龙头 rpg 方向失灵  @百度贴吧 ](https://tieba.baidu.com/p/7864541468)
{{< /details >}}