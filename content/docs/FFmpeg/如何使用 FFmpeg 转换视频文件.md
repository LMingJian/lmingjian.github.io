---
title: 如何使用 FFmpeg 转换视频文件
date: 2024-12-25T11:14:44+08:00
author: LiangMingJian
---

# 视频格式转换

```
ffmpeg -i input.avi output.mp4
ffmpeg -i input.mp4 output.ts
```

# 视频剪切

```
ffmpeg -ss 00:00:15 -t 00:00:05 -i input.mp4 -vcodec copy -acodec copy output.mp4
```

-ss 表示开始切割的时间，-t 表示要切多少。

注意一个问题，ffmpeg 在切割视频的时候无法做到时间绝对准确，因为视频编码中关键帧 I 帧和跟随它的 B 帧、P 帧是无法分割开的，否则就需要进行重新帧内编码，会让视频体积增大。所以，如果切割的位置刚好在两个关键帧中间，那么 ffmpeg 会向前/向后切割，所以最后切割出的 chunk 长度总是会大于等于应有的长度。

# 码率控制

码率控制对于在线视频比较重要，因为在线视频需要考虑其能提供的带宽。那么，什么是码率？很简单：bitrate = file size / duration，比如一个文件 20.8 M，时长 1 分钟，那么，码率就是：biterate = 20.8 M bit / 60s = 20.8 * 1024 * 1024 * 8 bit / 60s = 2831Kbps

一般音频的码率只有固定几种，比如是 128Kbps， 那么，video的就是：video biterate = 2831Kbps - 128Kbps = 2703Kbps。

ffmpeg 使用 -minrate、-b:v、-maxrate 来控制码率

-b:v 主要是控制平均码率。 比如一个视频源的码率太高了，有 10 Mbps，文件太大，想把文件弄小一点，但是又不破坏分辨率。

```
ffmpeg -i input.mp4 -b:v 2000k output.mp4
```

ffmpeg 官方 wiki 建议，设置 b:v 时，同时加上 -bufsize-bufsize 用于设置码率控制缓冲器的大小，设置的好处是，让整体的码率更趋近于希望的值，减少波动。

```
ffmpeg -i input.mp4 -b:v 2000k -bufsize 2000k output.mp4
```

-minrate、-maxrate 设置码率波动不要超过一个阈值。

```
ffmpeg -i input.mp4 -b:v 2000k -bufsize 2000k -maxrate 2500k output.mp4
```

# 视频编码格式转换

视频的编码是MPEG4，转换为H264编码：

```
ffmpeg -i input.mp4 -vcodec h264 output.mp4
ffmpeg -i input.mp4 -vcodec mpeg4 output.mp4
```

如果 ffmpeg 编译时，添加了外部的 x265 或者 x264 编码器，那也可以用外部的编码器来编码转换：

```
ffmpeg -i input.mp4 -c:v libx265 output.mp4
ffmpeg -i input.mp4 -c:v libx264 output.mp4
```
