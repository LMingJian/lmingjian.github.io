---
title: 如何安装 CUDA 并使用 PyTroch
date: 2024-12-25T10:13:47+08:00
author: LiangMingJian
---

# CUDA 是什么

CUDA 是 NVIDIA 提供的，用于 N 卡，集开发、优化和部署一体的一个 GPU 工具包。借助 CUDA 工具包，开发者可以通过 C/C++/Python 等编程语言**调用 GPU 进行通用计算**。CUDA 是现在运行各大 AI 模型的必备工具。

# CUDA 的下载

CUDA 可以前往 NVIDIA 官网下载安装：

[ CUDA Toolkit Archive 下载 ](https://developer.nvidia.com/cuda-toolkit-archive)

![](_images/drawingbed/img/Pasted%20image%2020251204191235.png)

**特别注意的是，不同版本的 CUDA 支持的最低版本显卡驱动各不相同，在安装时应当关注目标设备显卡驱动是否满足所下载 CUDA 的要求。**

对应版本 CUDA 所支持的最低驱动可以通过 NVIDIA 官网文档查找：

[ CUDA Release Notes ](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)

![](_images/drawingbed/img/Pasted%20image%2020251204191631.png)

主机上的 N 卡驱动信息可在 NVIDIA 控制面板中查看：

![](_images/drawingbed/img/Pasted%20image%2020251204193110.png)

# CUDA 的安装

**开始安装时**，安装程序会要求输入一个地址，这个是临时解压地址，在安装结束后会自动删除（默认情况下直接点击确定即可）。

![](_images/drawingbed/img/Pasted%20image%2020251204193957.png)

![](_images/drawingbed/img/Pasted%20image%2020251204194044.png)

解压完成即可进入安装过程。

![](_images/drawingbed/img/Pasted%20image%2020251204194410.png)

![](_images/drawingbed/img/Pasted%20image%2020251204194524.png)

**安装过程中，推荐使用自定义安装，仅勾选 CUDA 项**。

![](_images/drawingbed/img/Pasted%20image%2020251204194557.png)

![](_images/drawingbed/img/Pasted%20image%2020251204194859.png)

**同时，注意去除勾选 CUDA 项里面的 Visual Studio Integration**，这个是 Visual Studio 的插件，一般我们不需要。

![](_images/drawingbed/img/Pasted%20image%2020251204195031.png)

完成上述配置后，点击下一步，选择安装地址。

![](_images/drawingbed/img/Pasted%20image%2020251204195121.png)

再点击下一步，等待 CUDA 安装完成。

![](_images/drawingbed/img/Pasted%20image%2020251204195230.png)

![](_images/drawingbed/img/Pasted%20image%2020251204195847.png)

![](_images/drawingbed/img/Pasted%20image%2020251204195858.png)

**在 CUDA 安装完成后**，可以在终端执行以下命令检查（若出现版本号则安装成功）：

```shell
nvcc -V
```

![](_images/drawingbed/img/Pasted%20image%2020251204195934.png)

> 拓展阅读：GPU 显卡算力也可以通过 NVIDIA 官网进行查看。

> [ RTX 20 系以上的算力 ](https://developer.nvidia.com/cuda-gpus)

> [ 其他旧时代显卡的算力 ](https://developer.nvidia.com/cuda-legacy-gpus)

![](_images/drawingbed/img/Pasted%20image%2020251204191307.png)

# PyTroch 是什么

PyTroch 是 Meta（原 Facebook）开源的一个 Python 深度学习框架，其专为 AI 研究设计，支持开发者使用 GPU 和 CPU 进行深度学习。

在通过 Python 运行 AI 模型时，该框架是不可缺少的。

# PyTroch 的安装

**PyTroch 的安装需要 Python 3.9 以上的版本**，如果是 Preview 版本，则需要 Python 3.10 以上的版本。

**PyTroch 的安装还需要关注 CUDA 版本**，不同版本的 CUDA，需要的 PyTroch 版本也各不相同。

PyTroch 版本与 CUDA 版本的对应关系可以在其官网中找到，同时，对应的安装命令也可以在官网中找到：

[ Install PyTorch ](https://pytorch.org/)

![](_images/drawingbed/img/Pasted%20image%2020251204192709.png)

特别的，其他旧版本的 PyTroch 可以在以下地址中找到：

> 特别注意，旧版本的 PyTroch 支持的 Python 版本是有限的，过新版本的 Python 会无法安装，一般建议使用 Python 3.9

[ previous versions of PyTorch ](https://pytorch.org/get-started/previous-versions/)

![](_images/drawingbed/img/Pasted%20image%2020251204192838.png)

# PyTroch 加速下载

特别的，由于 PyTroch 官方地址在外网，通过 pip 下载时可能会很慢，因此我们可以寻找第三方镜像地址，或者直接通过第三方下载软件访问软件库直接下载 whl 文件手动安装。

**寻找第三方镜像**

阿里云开源镜像站提供大部分 Pytorch Wheels 的镜像下载，对应的连接如下：

[ Pytorch-wheels 镜像 ](https://developer.aliyun.com/mirror/pytorch-wheels)

![](_images/drawingbed/img/Pasted%20image%2020251205102347.png)

一般情况下，可以按介绍所说的将官方提供的下载地址替换为镜像地址来进行下载，比如：

```python
# 原下载命令
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu126

# 修改后的下载命令
pip install torch torchvision --index-url https://mirrors.aliyun.com/pytorch-wheels/cu126
```

**特别的，如果上述修改后的命令无法使用，则说明因为 PyTroch 修改了版本库结构，但镜像没有同步，导致下载无法找到软件包，此时就需要用户使用下面的第二种方式，手动的下载 whl 包并自行安装。**

**直接通过第三方软件下载**

比如下述的官方的下载安装命令：

```python
# CUDA 11.1 
pip install torch==1.10.1+cu111 torchvision==0.11.2+cu111 torchaudio==0.10.1 -f https://download.pytorch.org/whl/cu111/torch_stable.html
```

参数 `-f https://download.pytorch.org/whl/cu111/torch_stable.html` 指示了下载地址，开发者可通过访问这个地址来寻找需要的软件包，并点击下载。

参数 `torch==1.10.1+cu111 torchvision==0.11.2+cu111 torchaudio==0.10.1` 就是开发者需要下载的软件包。

在版本库中，软件包命名一般为 `torch-1.10.2%2Bcu111-cp39-cp39-win_amd64.whl` 的格式，其各部分内容含义分别是：

- torch 版本 1.10.2
- %2B 是 + 的转义
- cu111 对应 CUDA 11.1
- cp39-cp39 对应支持的 Python 3.9
- win_amd64 则说明对应 Windows 64 位系统。

这里找到的软件正常也可以在阿里云镜像中找到，因此一般建议直接在阿里云镜像中搜索并下载。如果阿里云中没有对应镜像则只能到官网仓库下载。

特别的，如果某个软件包缺少对应的 Windows 版本，则应当同时降低其他所有软件版本以适配具有 Windows 版本的安装包（torch 官方并不会每个版本都打包的）。

比如上述命令中的 torchvision 0.11.2，其就缺少 Windows 安装包，根据 [issues](https://github.com/pytorch/vision/issues/8962) 的描述，开发者应当将版本降级至 0.10.1，同时根据旧版本 [previous versions of PyTorch](https://pytorch.org/get-started/previous-versions/) 中的部署连接，找出对应 torchvision 所支持的 torch，torchaudio，重新进行下载。

```python
# 新的下载 CUDA 11.1
pip install torch==1.9.1+cu111 torchvision==0.10.1+cu111 torchaudio==0.9.1 -f https://download.pytorch.org/whl/torch_stable.html
```

![](_images/drawingbed/img/Pasted%20image%2020251205102730.png)

下载后的 whl 文件可以通过以下命令进行安装：

```python
pip install xxx.whl
```

**最后，重点提醒，PyTroch 官方给出的部署命令中的安装版本是经过测试的安装版本，在安装时要确保所安装的软件版本与官方一致，以避免运行失败的问题。**

# PyTroch 测试

在 PyTroch 安装成功后，可以执行以下命令进行测试（此时使用 CPU 计算）。

> PyTroch 是运行时需要大量内存，请确保运行时系统内存足够，或设置足够的虚拟内存。

```python
> python
> import torch
> x = torch.rand(5, 3)
> print(x)
tensor([[0.3380, 0.3845, 0.3217],
        [0.8337, 0.9050, 0.2650],
        [0.2979, 0.7141, 0.9069],
        [0.1449, 0.1132, 0.1375],
        [0.4675, 0.3947, 0.1426]])
```

通过 `torch.cuda.is_available()` 启用 CUDA 来使用 GPU 加速（此时使用 GPU 计算，若程序正常运行无报错，则安装成功）。

> 只有当安装的 PyTroch 版本是 cuxx 格式时，才能使用 CUDA，不携带 cuxx 的 PyTroch 版本只能使用上述的 CPU 运算。

```python
> torch.cuda.is_available()
> x = torch.rand(5, 3)
> print(x)
tensor([[0.3380, 0.3845, 0.3217],
        [0.8337, 0.9050, 0.2650],
        [0.2979, 0.7141, 0.9069],
        [0.1449, 0.1132, 0.1375],
        [0.4675, 0.3947, 0.1426]])
```
