---
title: 如何在 X86 Windows 上虚拟 ARM 服务器
date: 2024-12-25T10:32:46+08:00
author: LiangMingJian
---

# 所需软件

- ARM 系统镜像（比如 UOS_aarch64）
- QEMU（ [https://qemu.weilnetz.de](https://qemu.weilnetz.de/) ） 
- UEFI 引导文件 QEMU_EFI.fd（ [http://releases.linaro.org/components/kernel/uefi-linaro](http://releases.linaro.org/components/kernel/uefi-linaro) ）
- TAP-Windows 虚拟网卡（ [https://tap-windows.updatestar.com](https://tap-windows.updatestar.com/) ）

# 准备

- 下载上述所有文件，并安装 QEMU，TAP-Windows
- 将 QEMU 的安装文件夹路径添加进入环境变量，比如：D:\Program Files\qemu
- 新建文件夹 D:\qvm，存放数据
- 将文件 QEMU_EFI.fd 与 ARM 系统镜像移入 D:\qvm

# 配置 NAT 网络

在网络适配器管理中修改新装的网络适配器的名称为一个英文名称，如 Tap0

- 控制面板\网络和 Internet\网络和共享中心\更改适配器设置

![](_images/drawingbed/img/Pasted%20image%2020251212141214.png)

将物理网卡的网络状态分享给虚拟网卡 Tap0

使用这种模式时，Tap0 通过 NAT 技术通过物理网卡访问外网，即连接 Tap0 的设备能访问外网，但外网不能访问这个设备，只有 Windows 自己可以访问这个设备。

- 右键 > 属性 > 共享 > 允许其他用户通过此连接（网络填虚拟网卡名称）
- 如果连接失败或创建失败，尝试卸载虚拟网卡，重启电脑后再次尝试

![](_images/drawingbed/img/Pasted%20image%2020251212141257.png)

![](_images/drawingbed/img/Pasted%20image%2020251212141306.png)

# 配置桥接网络

在网络适配器管理中修改新装的网络适配器的名称为一个英文名称，如 Tap0

- 控制面板\网络和 Internet\网络和共享中心\更改适配器设置

![](_images/drawingbed/img/Pasted%20image%2020251212141331.png)

创建网桥，连接物理网卡和虚拟网卡 Tap0

使用这种模式时，Windows 会充当一个交换机，帮助物理网卡和虚拟网卡连接到外网的路由器上，此时网络联通后，物理网卡和虚拟网卡会有同一个网段的 IP，连接虚拟网卡的设备可以自由访问外网，也可以被外网访问

- 按住 Ctrl > 选择物理网卡和虚拟网卡 > 右键 > 桥接
- 当桥接完成后，适配器管理中会出现新的网桥，网桥成功连接上网络后，代表桥接完成
- 桥接模式与 NAT 模式不共存，使用桥接模式时，请先关闭物理网卡的共享
- 使用桥接模式时，请确保物理网卡和虚拟网卡，IP 和 DNS 都是自动获取
- 如果连接失败或创建失败，尝试卸载虚拟网卡，重启电脑后再次尝试

![](_images/drawingbed/img/Pasted%20image%2020251212141406.png)

![](_images/drawingbed/img/Pasted%20image%2020251212141413.png)

# 安装虚拟机

打开终端或 CMD，定位工作目录到安装文件夹，如 D:/qvm，开始执行安装命令：

```
cd D:/qvm
```

创建存储空间

```
qemu-img create -f qcow2 D:\qvm\Kylin-Server-10-SP2-aarch64.img 80G
# 或 qemu-img.exe create -f qcow2 d:\qvm\Kylin-Server-10-SP2-aarch64.qcow2 80G
# 使用时，可自行替换存储文件名，以及容量大小
```

生成虚拟机

```
qemu-system-aarch64 -m 4000 -cpu cortex-a72 -smp 4,cores=4,threads=1,sockets=1 -M virt -bios D:\qvm\QEMU_EFI.fd -net nic -net tap,ifname=tap -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64-Release-Build09-20210524.iso,id=cdrom,media=cdrom -device virtio-scsi-device -device scsi-cd,drive=cdrom -drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64.img,id=hd0 -device virtio-blk-device,drive=hd0
```

注意，上述命令需要根据需求，修改以下参数：

`-m 4000` 表示分配给虚拟机的内存最大 4000MB，可以直接使用 -m 4G

`-cpu cortex-a72` 指定CPU类型，还可以选择 cortex-a53、cortex-a57 等

`-smp 4,cores=4,threads=1,sockets=1` 指定虚拟机最大使用的 CPU 核心数

`-M virt` 指定虚拟机类型为 virt，具体支持的类型可以使用 qemu-system-aarch64 -M help 查看

`-bios D:\qvm\QEMU_EFI.fd` 指定 UEFI 固件文件

`-net tap,ifname=tap` 启用网络，ifname=tap中的 tap 请修改为前面步骤中虚拟网卡的名称

`-device nec-usb-xhci -device usb-kbd -device usb-mouse` 启用 USB 鼠标等设备

`-device VGA` 启用 VGA 视图，提供图形化控制界面

`-drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64-Release-Build09-20210524.iso,id=cdrom,media=cdrom` 指定使用的镜像文件，请修改为对应 ARM 系统镜像

`-device virtio-scsi-device -device scsi-cd,drive=cdrom` 指定光驱硬件类型

`-drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64.img` 指定硬盘镜像文件，修改为前面步骤中创建的存储镜像

# 日常启动

在安装完 ARM 系统后，便可以将 `-drive if=none,file=,id....` 的 file 指向空，然后将命令保存到 bat 文件里面，在日常使用。

```
qemu-system-aarch64 -m 4000 -cpu cortex-a72 -smp 4,cores=4,threads=1,sockets=1 -M virt -bios D:\qvm\QEMU_EFI.fd -net nic -net tap,ifname=tap -device nec-usb-xhci -device usb-kbd -device usb-mouse -device VGA -drive if=none,file=,id=cdrom,media=cdrom -device virtio-scsi-device -device scsi-cd,drive=cdrom -drive if=none,file=D:\qvm\Kylin-Server-10-SP2-aarch64.img,id=hd0 -device virtio-blk-device,drive=hd0
```

# Ex 拓展阅读

在创建虚拟机前，可以使用 `qemu-system-aarch64 --machine help` 来查看系统支持的设备，然后在创建虚拟机的时候使用 `-M` 来进行设备的指定。

每种虚拟器都有其支持的 CPU 类型，可以通过 `qemu-system-aarch64 -cpu help` 来查看支持的 CPU，然后在创建虚拟机时使用 `-cpu` 来进行指定。
