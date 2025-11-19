---
title: 如何使用 Windows 自带的远程桌面访问无密码的 Windows
date: 2024-12-25T16:17:20+08:00
author: LiangMingJian
---

# 问题

 在 Windows 中，用户可以使用系统自带 RDP 远程工具来进行局域网间的远程桌面控制。
 
 需要注意，在一般情况下，无密码的 Windows 系统是禁止远程控制的，用户如果需要远程控制无密码的电脑，则需要在本地策略编辑器中开放相应权限。

# 解决

**第一步**

在 Windows 设置中搜索【远程桌面设置】，然后启用**允许远程连接到此电脑**。

![](/_images/drawingbed/img/202212061940070.png)

![](_images/drawingbed/img/Pasted%20image%2020251111102307.png)

**第二步**

在**远端目标电脑上**，通过`Win + R` 打开运行，并输入 `gpedit.msc` 打开本地策略编辑器。

![](/_images/drawingbed/img/202212061947039.png)

**第三步**

在本地策略编辑器中定位到`Windows 设置—>安全设置—>本地策略—>安全选项`，然后禁用**账户：使用空白密码的本地帐户只允许进行控制台登录**

![](/_images/drawingbed/img/202212061948974.png)

**第四步**

回到**本地电脑**，通过`Win + R` 打开运行，输入 `mstsc` 打开远程桌面连接，输入对应远端电脑的 IP，启动远程桌面。

![](_images/drawingbed/img/Pasted%20image%2020251111102540.png)

![](_images/drawingbed/img/Pasted%20image%2020251111102609.png)
