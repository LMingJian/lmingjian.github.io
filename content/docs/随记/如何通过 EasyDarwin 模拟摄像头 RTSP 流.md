---
title: 如何通过 EasyDarwin 模拟摄像头 RTSP 流
date: 2024-12-25T10:31:16+08:00
author: LiangMingJian
---

# 需求

将 MP4 等视频文件模拟成摄像头的 RTSP 流，被其他应用拉流获取。

# 方法

通过新版的 EasyDarwin 一站式实现 MP4 文件的转换和 RTSP 流模拟。

# 步骤

1.前往 [EasyDarwin 官网](https://www.easydarwin.org/) 下载对应软件。

![](/_images/drawingbed/img/202401031026602.png)

2.解压，在文件夹中双击 `EasyDarwin.exe` 文件启动服务。

![](/_images/drawingbed/img/202401031027829.png)

3.在浏览器中访问 127.0.0.1:10086，使用默认账号密码 admin-admin 登录平台。

4.在【点播服务】中上传视频文件，平台会自动进行转码。

![](/_images/drawingbed/img/202401031030096.png)

5.在【直播服务】>【直播管理】中，创建直播间，填写信息，选择拉流，点播资源，开启直播。

![](/_images/drawingbed/img/202401031031862.png)

6.点击【播放】，即可在弹窗中看到对应的 RTSP 地址，可使用 VLC 进行拉流验证。

![](/_images/drawingbed/img/202401031033394.png)

![](/_images/drawingbed/img/202401031036466.png)

{{< details "参考文件" >}} 
1：[ 视频文件+EasyDarwin 模拟 RTSP 流 ](https://blog.csdn.net/xiejiashu/article/details/134633513)
{{< /details >}}
