---
title: Adb 调试工具
date: 2020-09-22
author: LMingJian
---

## 1.Adb 的使用

Adb （Android 调试桥）是一种功能多样的命令行工具，可让您与设备进行通信。此外，Adb 命令还可用于执行各种设备操作。

Adb 通常会随 Android SDK 一同安装，常见于 `SDK\Android\platform-tools` 内。下面是一些常用的 Adb 命令。

```bash
# 环境 Huawei TRT-AL00A
adb devices # 设备
adb shell wm size # 长宽
adb shell getevent -p  # 监听事件
adb shell # shell
>>> HWTRT-Q:/ $ getevent /dev/input/event4 # 按键事件
>>> HWTRT-Q:/ $ exit
adb shell getevent /dev/input/event4 # 按键事件
adb shell dumpsys # 获取当前运行的服务
adb shell dumpsys battery  # 获取设备电池信息
adb shell dumpsys cpuinfo
adb shell dumpsys meminfo
# 要获取具体应用的内存信息，可加上包名
adb shell dumpsys meminfo PACKAGE_NAME
# 获取某个包的信息：
adb shell dumpsys package PACKAGE_NAME
```

## 2.使用 Adb 模拟屏幕点击

### 1.获取屏幕事件（ event） 的宽高

```bash
adb shell 
# 进入 shell 模式
>>> HWTRT-Q:/ $ getevent -p
# 获取所有输入事件信息，找到宽（0035）和高（0036）这两个属性，这代表 event 中的宽高
>>> 0035  : value 0, min 0, max 719, fuzz 0, flat 0, resolution 0
>>> 0036  : value 0, min 0, max 1279, fuzz 0, flat 0, resolution 0
# 显然这里 719 X 1279 为事件宽高
# 获取到事件宽高的文件位置
add device 3: /dev/input/event4
```

### 2.计算比例

```bash
rateW = 720(手机屏幕的宽) / 719(event里0035的max) = 0.99
rateH = 1280(手机屏幕的高) / 1279(event里0036的max) = 0.99
# 理论上大部分手机都不需要换算
```

### 3.获取点击坐标

```bash
# 在 shell 模式下输入
>>> getevent /dev/input/event4
# 监控点击输入，关注 0035 与 0036 两个属性，然后换算到屏幕坐标
>>> 0003 0035 00000264
>>> 0003 0036 00000217
>>> 0003 0035 000001cb
>>> 0003 0036 0000049b
# 这里注意 0035、0036 的属性都是 16 进制需要换算为 10 进制 0x264=612  0x217=535 
# screenW = width*rateW
# screenH = height*rateH
# 获得屏幕坐标(screenW,screenH)
```

### 4.模拟按键

```bash
>>> input tap 612 535
# 点击(612,535)这个位置
```

{{< details "参考文件" >}} 
1：[ adb @Android Studio用户指南 ](https://developer.android.google.cn/studio/command-line/adb?hl=zh-cn)
{{< /details >}}