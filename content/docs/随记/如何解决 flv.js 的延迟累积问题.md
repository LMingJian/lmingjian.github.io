---
title: 如何解决 flv.js 的延迟累积问题
date: 2025-05-08T19:02:16+08:00
author: LiangMingJian
---

# 需求

flv.js 实现 FLV 视频播放的原理是从服务器获取到 FLV 格式的音视频数据后，通过原生的 JS 去解码 FLV 数据，再通过 Media Source Extensions API 喂给原生 HTML5 Video 标签。这里经过
从服务器获取 FLV，将解码转换后的数据再喂给 Video 标签两个步骤。

从上面这两个步骤不难看出，flv.js 有一个最大的问题，就是延迟问题。

一方面是直播端的延迟（服务器获取 FLV），一方面是浏览器的延迟（JS 解码），而且浏览器的延迟如果不做特殊处理，会造成延时累积的问题，对直播的实时性影响很大。

# 方案一：修改 config 配置

```json
{
    enableWorker: true, // 启用分离的线程进行转换   
    enableStashBuffer: false, // 关闭IO隐藏缓冲区    
    stashInitialSize: 128, // 减少首帧显示等待时长 
}
```

- 开启 flv.js 的 Worker，多线程运行 flv.js 提升解析速度可以优化延迟
- 关闭 buffer 缓存，这个选项可以明显地降低延迟，缺点就是由于关闭了 buffer 缓存，网络不好的时候可能会出现 loading 加载
- 调低 IO 缓冲区的初始尺寸，减少首帧显示的等待时长

# 方案二：追帧

```javascript
videoElement.addEventListener("progress", () => {

    let end = player.buffered.end(0); //获取当前buffered值(缓冲区末尾)
    let delta = end - player.currentTime; //获取buffered与当前播放位置的差值
    
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
4. 追帧可以直接更新当前的时间：`this.player.currentTime = this.player.buffered.end(0) - 1`，但这种做法的缺点是如果频繁触发会导致跳帧，观感较差
5. 追帧也可以使用调快播放速度的方式：`this.videoElement.playbackRate = 1.1`，优点是稳定，缺点是如果 `delta` 值过大，通过这种方式追得太慢

{{< details "参考文件" >}} 
1：[ flv.js 的追帧、断流重连及实时更新的直播优化方案 - CSDN ](https://blog.csdn.net/gw5205566/article/details/131290591)
{{< /details >}}
