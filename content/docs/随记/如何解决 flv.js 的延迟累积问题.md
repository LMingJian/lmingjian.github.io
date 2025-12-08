---
title: 如何解决 flv.js 的延迟累积问题
date: 2025-05-08T19:02:16+08:00
author: LiangMingJian
---

# 需求

flv.js 实现 FLV 视频播放的原理是从服务器获取 FLV 格式的音视频数据后，通过原生的 JS 去解码 FLV 数据，再通过 Media Source Extensions API 喂给原生 HTML5 Video 标签。

在这个过程中，主要经过：从服务器获取 FLV 和将解码转换后的数据喂给 Video 标签，这两个步骤。不难看出，这两个步骤执行的快慢必然会导致延迟问题。

一方面是直播端的延迟（服务器获取 FLV），一方面是浏览器的延迟（JS 解码）。浏览器的延迟如果不做特殊处理，那最终就会造成延时累积的问题，对直播的实时性造成影响。

# 方案一：修改 config 配置

```json
{
    enableWorker: true, // 启用分离的线程进行转换   
    enableStashBuffer: false, // 关闭IO隐藏缓冲区    
    stashInitialSize: 128, // 减少首帧显示等待时长 
}
```

- 开启 flv.js 的 Worker，多线程运行 flv.js 提升解析速度可以优化延迟
- 关闭 buffer 缓存，提高视频的加载效率
- 调低 IO 缓冲区的初始尺寸，减少首帧显示的等待时长

# 方案二：追帧

```javascript
videoElement.addEventListener("progress", () => {

    let end = player.buffered.end(0); // 获取当前 buffered 值(缓冲区末尾)
    let delta = end - player.currentTime; // 获取 buffered 与当前播放位置的差值
    
    // 延迟过大，通过跳帧的方式更新视频
    if (delta > 10 || delta < 0) {
        this.player.currentTime = this.player.buffered.end(0) - 1;
        return;
    }
    
    // 追帧
    if (delta > 1) {
        videoElement.playbackRate = 1.1;
    } else {
        videoElement.playbackRate = 1;
    }
    
});
```

通过判断缓冲区末尾的 buffer 值与当前播放时间的差值，来进行追帧操作。

1. 首先，在 progress 事件，或者定时器中进行追帧逻辑
2. 判断 buffer 的差值 `delta`
3. 如果 `delta` 值大于某个设定的值，则进行追帧操作

追帧操作可以使用以下两种：

- **直接更新时间**：`this.player.currentTime = this.player.buffered.end(0) - 1`，需要注意，这种做法的缺点是，如果频繁触发，则会导致跳帧，观感较差
- **加快播放速度**：`this.videoElement.playbackRate = 1.1`，这种方法的优点是稳定，但缺点是如果 `delta` 值过大，需要较长的时间才能完成追帧

——————————

> [ flv.js 的追帧、断流重连及实时更新的直播优化方案 - CSDN ](https://blog.csdn.net/gw5205566/article/details/131290591)
