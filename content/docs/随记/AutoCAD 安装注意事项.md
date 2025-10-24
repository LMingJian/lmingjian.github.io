---
title: AutoCAD 安装注意事项
date: 2025-10-24T16:16:43+08:00
author: LiangMingJian
---

> 以下内容针对版本 2020，但大概率其他版本也适用

# 卸载后重新安装失败 1603

在卸载后重新安装 AutoCAD 时，可能会出现安装失败，提示：

**“安装未完成，某些产品无法安装（错误代码 1603）”**

![](_images/drawingbed/img/Pasted%20image%2020251024162049.png)

此时，要解决这个问题，首先确保以下两步内容完成。

**第一**

在【控制面板 > 程序 > 程序与功能】中，卸载所有 AutoCAD 相关软件。

**第二**

在【C:\Program Files (x86)\Common Files\Autodesk Shared\AdskLicensing】中，找到卸载软件 uninstall.exe，右键以管理员身份运行，卸载 AdskLicensing 组件。

在卸载完成后，返回【C:\Program Files (x86)\Common Files\】，删除整一个【Autodesk Shared】文件。

以上，在完成上述操作后，你就可以重新安装 AutoCAD，此时安装会正常进行。

> 注册是跟电脑走的，卸载重新安装后，只要产品代码没变化，注册信息还是保存的。

> 相关资料：[ 安装Autodesk 2020以及更高软件软件提示1603错误 ](https://www.autodesk.com.cn/support/technical/article/caas/sfdcarticles/sfdcarticles/kA93g0000000Bg8.html)

# 注册机必须使用管理员身份运行

注册机在使用时，必须【右键 > 以管理员身份运行】，否则程序会因为权限不足导致无法执行 Path 操作。

# 重启后注册又失败了怎么解决

AutoCAD 的注册是以 Windows 服务存在于系统中的，因此用户需要检查【右键我的电脑 > 管理 > 计算机管理 > 服务> **Autodesk Desktop Licensing Service**】这里的服务是否是**自动启动**的，如果不是，则应当将其置为自动启动。

注意，某些杀毒软件可能会将这个服务项作为不好的开机启动项关闭，此时需要用户到杀毒软件中开放。

![](_images/drawingbed/img/Pasted%20image%2020251024163937.png)

![](_images/drawingbed/img/Pasted%20image%2020251024164005.png)

![](_images/drawingbed/img/Pasted%20image%2020251024164029.png)

> 相关资料：[ 在 Windows 上启动 Autodesk 2020 或更高版本软件时显示“许可检出超时。您要执行什么操作 ](https://www.autodesk.com.cn/support/technical/article/caas/sfdcarticles/sfdcarticles/kA93g0000000gGL.html)