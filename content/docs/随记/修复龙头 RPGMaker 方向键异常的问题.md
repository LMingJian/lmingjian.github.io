---
title: 修复龙头 RPGMaker 方向键异常的问题
date: 2024-12-25T10:36:45+08:00
author: LiangMingJian
---

# BUG 描述

在游玩龙头 RPGMaker 制作的游戏时，突然出现无法使用方向键进行控制的问题。

在启动游戏后，人物会自动的往左上跑，一直输出向上和向左的信号。

# Resolution

初步怀疑是由于向日葵等远程软件的虚拟设备与 RPGMaker 存在冲突，导致出现上述故障，解决办法为卸载向日葵等远程软件。

检查 Windows 设备管理器，在人机接口设备中查看是否存在 HID virtual device kit standard 等设备，寻找安装该设备的程序进行卸载。

> 1：[ 龙头 rpg 方向失灵  @百度贴吧 ](https://tieba.baidu.com/p/7864541468)
