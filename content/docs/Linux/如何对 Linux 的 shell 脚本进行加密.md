---
title: 如何对 Linux 的 shell 脚本进行加密
date: 2024-12-25T13:55:08+08:00
author: LiangMingJian
---

# 需求

某些时候，我们不希望自己编写的 shell 脚本被人直接看到，或有些密码写进了脚本里面，此时我们就需要对脚本进行加密。

# 使用 gzexe 实现

gzexe 特点是不用安装，加解密极其简单，其本质是一个压缩工具，能起到隐藏，混淆文件内容的效果，但是如果加密文件本身被偷走，那别人也可以轻易破解。

```shell
# 加密
gzexe l.sh
# 解密
gzexe -d l.sh
```

# 使用 shc 实现

shc 需要下载源码进行安装，使用起来比 gzexe 复杂，其本质是一个编译工具，可以通过把 shell 脚本编译成可执行程序，来实现加密的效果。

```shell
# 加密，-v：显示加密过程，-f：需要加密的文件，-r：文件支持在其他服务器上使用
shc -v -r -f register.sh -o register
```

# Ex.安装 shc

- 1.访问项目：https://github.com/neurobin/shc
- 2.下载源代码
- 3.将源代码上传到服务器，分别执行`./configure`，`make`，`sudo make install`
- 4.安装完成
