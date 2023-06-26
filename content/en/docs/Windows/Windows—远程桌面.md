---
title: Windows 系统启动远程桌面
date: 2022-12-06
author: LM
---

## Question

如何使用 Windows 自带的远程工具进行远程桌面连接。

## Step

1. 在 Windows 设置中搜索【远程桌面设置】，并**允许远程连接到此电脑**

   ![](/images/drawingbed/img/202212061940070.png)

2. 在**远端电脑**通过`Win + R` 打开运行

3. 输入 `gpedit.msc` 打开本地策略编辑器

   ![](/images/drawingbed/img/202212061947039.png)

4. 定位：`Windows 设置—>安全设置—>本地策略—>安全选项`

5. **禁用：使用空白密码的本地帐户只允许进行控制台登录**

   ![](/images/drawingbed/img/202212061948974.png)

6. 在本地通过`Win + R` 打开运行`Win + R` 打开运行

7. 输入 `mstsc` 打开【远程桌面连接】，输入对应远端电脑的 IP，启动远程桌面

