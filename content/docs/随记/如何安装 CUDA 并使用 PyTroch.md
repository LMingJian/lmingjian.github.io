---
title: 如何安装 CUDA 并使用 PyTroch
date: 2024-12-25T10:13:47+08:00
author: LiangMingJian
---

# CUDA，PyTroch 是什么

CUDA 是开发、优化和部署支持 GPU 加速应用的一个工具包，借助 CUDA 工具包，您可以在经 GPU 加速的嵌入式系统、台式工作站、企业数据中心、基于云的平台和 HPC 超级计算机中开发、优化和部署应用。

PyTroch 是一个开源的 Python 深度学习框架，用于使用 GPU 和 CPU 进行深度学习。

# PyTroch 和 CUDA 的安装

在安装时，需要注意 PyTroch 与 CUDA 的版本对应关系，不同版本的 CUDA 需要使用不同的 PyTroch，而不同版本的 PyTroch 又依赖于不同的显卡算力。

因此，用户在安装 PyTroch 时，需要明确自己显卡的算力，自己显卡驱动支持的 CUDA，自己显卡算力支持的 PyTroch。

- 用户可以通过：https://developer.nvidia.com/cuda-gpus，查看显卡算力。
- 用户可以通过：https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html，查看显卡驱动支持的 CUDA。
- 用户可以在安装 PyTroch 后，执行`import torch; torch.cuda.get_arch_list();`，在结果 `['sm_37', 'sm_50', 'sm_60', 'sm_70']` 中查看显卡算力是否满足，不满足则降低 PyTroch 版本并重新安装。

访问 https://pytorch.org/get-started/locally/，在文档中会有目前最新版本 PyTroch 的安装指令，也提供链接前往旧版本下载地址，用户可以择优进行选择。

访问 https://developer.nvidia.com/cuda-toolkit-archive，可以下载所有版本的 CUDA 安装包，用户可以依据显卡择优进行选择。

安装 CUDA 时，安装程序会要求输入两个地址， 第一个是临时解压目录，用后删除，第二个是安装目录。安装过程中，应当使用自定义安装，仅勾选 CUDA 项，同时注意去除勾选 CUDA 项里面的 Visual Studio Integration。安装成功后，可以在命令行中执行`nvcc -V`进行检查。

# PyTroch 测试

在 PyTroch 安装成功后，可以执行以下命令进行测试。

```
> Python
> import torch
> x = torch.rand(5, 3)
> print(x)
tensor([[0.3380, 0.3845, 0.3217],
        [0.8337, 0.9050, 0.2650],
        [0.2979, 0.7141, 0.9069],
        [0.1449, 0.1132, 0.1375],
        [0.4675, 0.3947, 0.1426]])
```

通过`torch.cuda.is_available()`启用 CUDA 来使用 GPU 加速。

```
> torch.cuda.is_available()
> x = torch.rand(5, 3)
> print(x)
tensor([[0.3380, 0.3845, 0.3217],
        [0.8337, 0.9050, 0.2650],
        [0.2979, 0.7141, 0.9069],
        [0.1449, 0.1132, 0.1375],
        [0.4675, 0.3947, 0.1426]])
```
