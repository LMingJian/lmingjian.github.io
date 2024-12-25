---
title: 如何在 Linux 中挂载硬盘
date: 2024-12-25T13:55:41+08:00
author: LiangMingJian
---

# 使用 GPT 分区挂载

大数据盘的分区和文件系统格式化和小盘都存在差异。大盘必须采用 GPT 分区格式， 不能再采用小盘使用的 MBR 分区格式。**（无论大小盘，建议都使用 GPT 分区）**

- MBR 分区格式：最大支持 2 TB 的磁盘。 
- GPT 分区格式：最大支持 18 EB。

## 1.检查磁盘信息

- 使用 SSH 或 VNC 方式登录主机
- 执行`fdisk -l`，查看磁盘是否存在，以及磁盘目录

![](/_images/drawingbed/img/202412051038927.png)

## 2.磁盘分区

- 输入`parted /dev/sdc`，进入 parted 分区工具，对磁盘进行分区
- 创建磁盘标签 ( parted )： `mklabel`，设置标签格式为 `GPT`
- 查看分区状态 ( parted )：`p`
- 执行分区 ( parted )：`mkpart`
- 指定分区名称 Partition name \[]?：回车
- 指定分区类型 File system type \[ext2]?： `ext4`
- 指定起始位置 Start：`0G`
- 指定结束位置 End：`2190G`
- 显示分区信息 ( parted )：`p`

![](/_images/drawingbed/img/202412051042869.png)

## 3.EXT4 系统格式化

- 输入`mkfs.ext4 -T largefile /dev/sdc1`，对设备 /dev/sdc1 进行格式化

![](/_images/drawingbed/img/202412051043794.png)

## 4.挂载目录

- 使用`cd /`进入根目录，创建目录地址`mkdir test`
- 输入`mount -t ext4 /dev/sdc1 /test`。挂载目录到 test 文件夹下
- 如果是挂载已有目录，比如 home，输入`mount -t ext4 /dev/sdc1 /home`
- 输入`df -h`查看当前盘信息

![](/_images/drawingbed/img/202412051052543.png)

![](/_images/drawingbed/img/202412051052085.png)

## 5.自动挂载

使用 mount 操作只能将硬盘临时挂载，重启主机之后，挂载信息就会丢失，为了保证挂载信息长期有效，需要对 fstab 文件进行配置。

- 输入`blkid /dev/sdc1`获取操作盘的 UUID 信息
- 执行`vi /etc/fstab`，按下`i`编辑 fstab 文件
- 将`UUID=******************** /test  ext4 defaults 1 2`添加至文本末端，然后再按`ESC`键，输入`:wq`保存文件
- 重启主机后，输入`df -h`可查看磁盘已经自动挂载。

![](/_images/drawingbed/img/202412051055649.png)

![](/_images/drawingbed/img/202412051055524.png)

![](/_images/drawingbed/img/202412051055967.png)

{{< details "参考文件" >}} 
1：[  初始化云硬盘 @移动云 ](https://ecloud.10086.cn/op-help-center/doc/article/48750)
{{< /details >}}

