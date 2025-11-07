---
title: Linux Command：tar
date: 2024-12-25T13:58:34+08:00
author: LiangMingJian
---

# 概述

在 Linux 中，打包和压缩是两个概念。

这是因为 Linux 中很多压缩程序只能针对一个文件进行压缩，因此当你想要压缩一大堆文件时，你得先将这一大堆文件先打成一个包，然后再用压缩程序进行压缩。

- **打包是指将一大堆文件或目录变成一个总的文件**
- **压缩则是将一个大的文件通过一些压缩算法变成一个小文件**

tar 命令本质是一个打包指令，为 linux 的文件和目录创建档案。

利用 tar 命令，可以把一大堆的文件和目录全部打包成一个文件，然后再通过其他工具参数的使用，将这个打包文件进行压缩。

# tar 支持的参数

‌**主选项（必须三选一使用）**‌：

- `-c`：创建新的归档文件
- `-x`：从归档文件中提取文件
- `-t`：列出归档文件的内容

**‌辅助功能选项‌（可选）：**

- `-z`：使用 gzip 进行压缩或解压（生成 .tar.gz 文件）
- `-j`：使用 bzip2 进行压缩或解压（生成 .tar.bz2 文件）
- `-J`：使用 xz 进行压缩或解压（生成 .tar.xz 文件）
- `-v`：显示操作过程的详细信息
- `-f`：指定归档文件名，必须是最后一个参数
- `-C`：解压到指定目录
- `--exclude`：排除特定文件或目录

# 以 .tar 格式打包文件

**打包**

```bash
tar -cvf FileName.tar DirName
```

**解包**

```bash
tar -xvf FileName.tar
```

# 使用 tar 解压文件

在现代版本中，`tar`命令支持自动识别压缩格式，因此可以直接使用 `xf` 参数进行解压，而不用指定解压工具。

```bash
tar -xf archive.tar.xz
tar -xf archive.tar.gz
```

# 以 .gz 格式打包并压缩文件

**压缩**

```bash
tar -zcvf FileName.tar.gz DirName
```

**加密压缩**

```bash
tar -zcvf - file | openssl des3 -salt -k password -out /path/to/file.tar.gz
```

**解压**

```bash
tar -zxvf FileName.tar.gz
```

**加密解压**

```bash
openssl des3 -d -k password -salt -in FileName.tar.gz | tar zxvf -
```

# 以 .bz2 格式打包并压缩文件

**压缩**

```bash
tar -jcvf FileName.tar.bz2 DirName
```

**解压**

```bash
tar -jxvf FileName.tar.bz2
tar -jxvf FileName.tar.bz
```

# 以 .xz 格式打包并压缩文件

**压缩**

```bash
tar -Jcvf FileName.tar.xz DirName
```

**解压**

```bash
tar -Jxvf FileName.tar.xz
```

# 其他的压缩工具

```bash
# .zip
unzip FileName.zip           # 解压
zip FileName.zip DirName     # 压缩

# .rar
rar -x FileName.rar           # 解压
rar -a FileName.rar DirName   # 压缩

# .gz
gzip -d FileName.gz    # 解压
gzip FileName          # 压缩

# .bz2
bzip2 -d FileName.bz2   # 解压
bzip2 -z FileName       # 压缩

# .xz
xz -d filename.xz  # 解压
xz filename        # 压缩
```
