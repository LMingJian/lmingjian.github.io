---
title: Windows Rust 安装手册
date: 2025-12-15T09:56:01+08:00
author: LiangMingJian
---

# 前言

在部署 [ Akegarasu 秋葉杏 ](https://github.com/Akegarasu) 的 Lora 训练项目 [ Lora Scripts ](https://github.com/Akegarasu/lora-scripts) 时，项目依赖 [ safetensors ](https://github.com/huggingface/safetensors) 要求设备应当具备 Rust 环境。受限于国内网络影响，如果直接通过 `pip install safetensors`，程序会因为网络受限导致无法下载 Rust，进而无法正常安装。因此，需要先部署 Rust，并配置相应的国内镜像地址后，再执行项目部署。

# Windows Rust 安装

**第一步，下载安装工具** `rustup-init.exe`

前往 Rust 官网下载页 [ Install Rust ](https://rust-lang.org/tools/install/)，按设备型号选择下载相应的安装工具。

![](_images/drawingbed/img/Pasted%20image%2020251215104039.png)

**第二步，设置国内的镜像加速地址**

rust 在安装时会通过 rustup 来进行网络下载，因此，我们需要设置 rustup 的国内镜像下载地址。rustup 的镜像配置需要通过设置环境变量 `RUSTUP_DIST_SERVER` 进行。

当前 rustup 在国内推荐使用的镜像加速地址有：**清华大学镜像站**，`RUSTUP_DIST_SERVER=https://mirrors.tuna.tsinghua.edu.cn/rustup`。

Windows 系统可以通过按下 `Win + S` 快捷键打开搜索，输入环境变量来跳转到设置弹窗，完成上述配置。

![](_images/drawingbed/img/Pasted%20image%2020251215112906.png)

![](_images/drawingbed/img/Pasted%20image%2020251215112952.png)

**第三步，运行** `rustup-init.exe`

rustup 的安装需要设备具有 MSVC 环境，因此在启动程序后，会要求用户选择是否安装 MSVC。

一般来说，推荐通过安装 Visual Studio 来间接安装 MSVC，以及其他配置依赖库。

![](_images/drawingbed/img/Pasted%20image%2020251215113836.png)

输入 1，回车，系统会自动下载 Visual Studio Installer 进行安装。

![](_images/drawingbed/img/Pasted%20image%2020251215114838.png)

![](_images/drawingbed/img/Pasted%20image%2020251215115156.png)

点击确认，开始安装 Visual Studio 和 MSVC。

在上述程序安装完成后，`rustup-init.exe` 会自动进行下一步，正式开始安装 `rust`。

输入 1，回车，系统开始安装 rust。（这个过程中，需要下载大量依赖，如果缺少上述镜像配置，则下载速度会较为缓慢）

![](_images/drawingbed/img/Pasted%20image%2020251215115721.png)

**最后，安装完成，通过以下指令查看是否可用**

```
# 编译器
rustc -V

# 包管理器
cargo -V

# 工具链管理器（用来切换和管理不同版本的 Rust）
rustup -V
```

![](_images/drawingbed/img/Pasted%20image%2020251215133902.png)

![](_images/drawingbed/img/Pasted%20image%2020251215134204.png)

# cargo 包管理器镜像配置

cargo 是 Rust 的包管理器，就像 Python 的 pip 一样。由于 cargo 的官方服务器也在国外，因此也需要使用镜像加速。

cargo 的镜像配置需要通过在配置文件夹 `.cargo` 中添加配置项 `config.toml` 进行实现。

文件夹 `.cargo` 默认在当前系统的用户文件夹内。 

![](_images/drawingbed/img/Pasted%20image%2020251215135150.png)

在 `config.toml` 文件内添加以下内容以配置镜像加速，详细的操作也参考：[ 清华大学镜像站-帮助 ](https://mirrors.tuna.tsinghua.edu.cn/help/crates.io-index.git/)

```
[source.crates-io]
replace-with = 'mirror'

[source.mirror]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
```
