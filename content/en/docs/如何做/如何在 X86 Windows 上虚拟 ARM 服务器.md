---
title: 如何在 X86 Windows 上虚拟 ARM 服务器
date: 2024-07-19
author: LM
---

## 1.所需软件

- ARM 系统镜像
- QEMU（ https://qemu.weilnetz.de ）
- UEFI 引导文件 QEMU_EFI.fd（ http://releases.linaro.org/components/kernel/uefi-linaro ）
- TAP-Windows 虚拟网卡（ https://tap-windows.updatestar.com ）

## 2.准备

- 安装以上所需文件
- 在网络适配器管理中修改新装的网络适配器的名称为 tap
- 将物理网卡的网络状态分享给虚拟网卡 tap，右键属性 > 共享 > 允许其他用户通过此连接
- 将 QEMU 的安装文件夹路径添加进入环境变量
- 新建文件夹 D:\qvm
- 将文件 QEMU_EFI.fd 与镜像文件移入 D:\qvm

## 3.安装

在 D:/qvm 中打开终端或 CMD，开始执行安装命令

```
qemu-img create -f qcow2 D:\qvm\Kylin-Server-10-SP2-aarch64.img 80G
# 注意路径不同
# 也可以使用 qemu-img.exe create -f qcow2 d:\qvm\Kylin-Server-10-SP2-aarch64.qcow2 80G
```

```
qemu-system-aarch64 -m 4000 -cpu cortex-a72 -smp 4,cores=4,threads=1,sockets=1 -M virt -bios D:\qvm\QEMU_EFI.fd -net nic -net tap,ifname=tap -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64-Release-Build09-20210524.iso,id=cdrom,media=cdrom -device virtio-scsi-device -device scsi-cd,drive=cdrom -drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64.img,id=hd0 -device virtio-blk-device,drive=hd0
# 注意路径不同

-m 4000 表示分配给虚拟机的内存最大4000MB，可以直接使用 -m 4G

-cpu cortex-a72 指定CPU类型，还可以选择cortex-a53、cortex-a57等

-smp 4,cores=4,threads=1,sockets=1 指定虚拟机最大使用的CPU核心数等

-M virt 指定虚拟机类型为virt，具体支持的类型可以使用 qemu-system-aarch64 -M help 查看

-bios D:\qvm\QEMU_EFI.fd 指定UEFI固件文件

-net tap,ifname=tap 启用网络功能（ifname=tap1212中的tap1212请修改为前面步骤中自己修改后的网卡名称）

-device nec-usb-xhci -device usb-kbd -device usb-mouse 启用USB鼠标等设备

-device VGA 启用VGA视图，对于图形化的Linux这条很重要！

-drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64-Release-Build09-20210524.iso,id=cdrom,media=cdrom 指定光驱使用镜像文件

-device virtio-scsi-device -device scsi-cd,drive=cdrom 指定光驱硬件类型

-drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64.img 指定硬盘镜像文件
```

```
# 安装成功后就无需再次指定iso文件启动了
qemu-system-aarch64 -m 4000 -cpu cortex-a72 -smp 4,cores=4,threads=1,sockets=1 -M virt -bios D:\qvm\QEMU_EFI.fd -net nic -net tap,ifname=tap -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=,id=cdrom,media=cdrom -device virtio-scsi-device -device scsi-cd,drive=cdrom -drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64.img,id=hd0 -device virtio-blk-device,drive=hd0
```


{{< details "参考文件" >}} 
1：[ win11 x86 系统部署 arm 架构的虚拟机  @阿干tkl ](https://blog.csdn.net/m0_58805648/article/details/131195276)
{{< /details >}}