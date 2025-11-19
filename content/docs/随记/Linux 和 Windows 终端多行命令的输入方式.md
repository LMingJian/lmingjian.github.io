---
title: Linux 和 Windows 终端多行命令的输入方式
date: 2025-10-11T14:07:44+08:00
author: LiangMingJian
---

# 需求

在编写一些 sh 脚本，bat 脚本时，往往在遇到一些过长命令时，输入的单行内容溢出了编辑器，需要进行左右滚动才能查看，此时代码的可读性非常差。

比如下面的 ffmpeg 命令：

```cmd
ffmpeg -hwaccel qsv -f dshow -framerate 20 -i audio="virtual-audio":video="virtual-screen" -ar 44100 -c:v h264_qsv -preset ultrafast -f rtsp rtsp://localhost:8554/mystream
```

为了便于后续查看维护，我们希望代码内容能在可视范围内显示，而不用左右移动，此时边需要通过分行符，将命令分行写入。

# Linux 的 bash 分行

在 Linux 中，bash 终端通过反斜杆 `\` 进行分行输入，比如：

```bash
ffmpeg -hwaccel qsv -f dshow \
-framerate 20 \
-i audio="virtual-audio":video="virtual-screen" \
-ar 44100 -c:v h264_qsv -preset ultrafast \
-f rtsp rtsp://localhost:8554/mystream
```

# Windows 的 cmd 分行

在 Windows 中，cmd 终端通过尖括号 `^` 进行分行输入，比如：

```cmd
ffmpeg -hwaccel qsv -f dshow ^
-framerate 20 ^
-i audio="virtual-audio":video="virtual-screen" ^
-ar 44100 -c:v h264_qsv -preset ultrafast ^
-f rtsp rtsp://localhost:8554/mystream
```

# Windows 的 powsershell 分行

在 Windows 中，powsershell 终端通过反引号 `` ` `` 进行分行输入，比如：

```powsershell
ffmpeg -hwaccel qsv -f dshow `
-framerate 20 `
-i audio="virtual-audio":video="virtual-screen" `
-ar 44100 -c:v h264_qsv -preset ultrafast `
-f rtsp rtsp://localhost:8554/mystream
```
