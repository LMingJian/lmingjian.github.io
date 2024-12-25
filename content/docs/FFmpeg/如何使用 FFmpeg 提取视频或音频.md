---
title: 如何使用 FFmpeg 提取视频或音频
date: 2024-12-25T11:14:39+08:00
author: LiangMingJian
---

# 提取音频

```
ffmpeg -i input.mp4 -acodec copy -vn output.aac
ffmpeg -i input.mp4 -acodec acc -vn output.aac
```

# 提取视频

```
ffmpeg -i input.mp4 -vcodec copy -an output.mp4
```
