---
title: 如何设置 Python pip 镜像源
date: 2025-09-03T16:32:36+08:00
author: LiangMingJian
---

# 永久修改

打开系统终端，执行以下的 pip 命令：

```python
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
```

> 重置：`pip config unset global.index-url`

**以验证生效的系统镜像地址（2025-09-03）**

- 阿里云镜像：`http://mirrors.aliyun.com/pypi/simple/`
- 腾讯云镜像：`https://mirrors.cloud.tencent.com/pypi/simple/`
- 校园网联合镜像：`https://chinanet.mirrors.ustc.edu.cn/pypi/simple/`
- 清华大学镜像：`https://pypi.tuna.tsinghua.edu.cn/simple/`
- ‌中国科学技术大学镜像：`https://mirrors.ustc.edu.cn/pypi/simple/`

> 可从此查看某些高校的镜像仓库使用帮助：[ PyPI 软件仓库镜像使用帮助 @MirrorZ Help ](https://mirror.nju.edu.cn/pypi/)

# 临时使用

在使用 pip 安装模块时，使用参数 `-i` 指定镜像地址：

```python
pip install numpy -i https://mirrors.aliyun.com/pypi/simple/
```
