---
title: MSVC cl 命令无法找到与 kernel32 丢失问题
date: 2025-12-15T13:56:04+08:00
author: LiangMingJian
---

# 前言

在部署 [ Akegarasu 秋葉杏 ](https://github.com/Akegarasu) 的 Lora 训练项目 [ Lora Scripts ](https://github.com/Akegarasu/lora-scripts) 时，项目依赖 [ safetensors ](https://github.com/huggingface/safetensors) 通过 `pip install safetensors` 下载安装时，需要使用 Rust 以及 MSVC 环境进行编译。

MSVC 在实际使用时，有概率出现以下问题。

# MSVC 编译工具 cl 提示不是内部或外部命令

![](_images/drawingbed/img/Pasted%20image%2020251215141344.png)

这是因为 MSVC 在安装后不会将该编译工具的路径填入环境变量，从而导致系统无法找到该便捷工具。

**解决方法**：结合 VS 安装路径 `C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.44.35207\bin\Hostx64\x64`，将其填入系统环境变量中即可。

Windows 系统可以通过按下 `Win + S` 快捷键打开搜索，输入环境变量来跳转到设置弹窗，完成上述配置。

![](_images/drawingbed/img/Pasted%20image%2020251215112906.png)

![](_images/drawingbed/img/Pasted%20image%2020251215141738.png)

配置完成后，在终端执行 `cl` 就不再报错了。

![](_images/drawingbed/img/Pasted%20image%2020251215141844.png)

# MSVC 编译时提示无法找到 kernel32

MSVC 在编译时，出现 `fatal error LNK1104: cannot open file "kernel32.lib"` 的报错提示，提示用户无法找到 kernel32 这一个依赖。这是因为 MSVC 在安装时缺少 Windows SDK 导致的问题。

**解决方法**：通过 Visual Studio Installer 修改 Visual Studio 安装内容，添加 Windows SDK 安装。

![](_images/drawingbed/img/Pasted%20image%2020251215142435.png)

![](_images/drawingbed/img/Pasted%20image%2020251215142650.png)

或通过 Windows SDK 官方网站手动下载安装包进行部署：[ Windows SDK ](https://developer.microsoft.com/zh-cn/windows/downloads/windows-sdk/#main)

![](_images/drawingbed/img/Pasted%20image%2020251215142927.png)
