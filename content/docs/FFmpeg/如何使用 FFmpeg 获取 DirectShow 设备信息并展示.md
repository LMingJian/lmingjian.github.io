---
title: 如何使用 FFmpeg 获取 DirectShow 设备信息并展示
date: 2024-12-25T11:14:34+08:00
author: LiangMingJian
---

# 概述

DirectShow 设备是指系统接入的视频音频设备，如摄像头，麦克风等。通过列出 DirectShow 设备，用户可以找到所有可使用的设备名称，然后通过 `ffplay` 进行播放。

# 列出所有设备

```
ffmpeg -list_devices true -f dshow -i dummy
```

输出：

```
[dshow @ 00000...] "screen-capture-recorder" (video)
[dshow @ 00000...]   Alternative name "@device_sw_..."
[dshow @ 00000...] "OBS Virtual Camera" (video)
[dshow @ 00000...]   Alternative name "@device_sw_..."
[dshow @ 00000...] "VoiceMeeter Output (VB-Audio VoiceMeeter VAIO)" (audio)
[dshow @ 00000...]   Alternative name "@device_cm_..."
[dshow @ 00000...] "virtual-audio-capturer" (audio)
[dshow @ 00000...]   Alternative name "@device_sw_..."
```

可以看到，这里有视频设备 screen-capture-recorder 和 OBS Virtual Camera，音频设备 VoiceMeeter Output 和 virtual-audio-capturer。

在使用时，可以直接使用设备名称，也可以使用 Alternative name 别名。

# 播放

```
ffplay -f dshow -i video="OBS Virtual Camera"
```

# 使用别名播放

```
ffplay -f dshow -i video="@device_sw_..."
```

# 限定分辨率

```
ffplay -s 1280x720 -f dshow -i video="OBS Virtual Camera"
ffplay -s 424x240 -f dshow -i video="OBS Virtual Camera"
```

# 同名设备指定

```
ffmpeg -f dshow -video_device_number 0 -i video="USB Video"
```
