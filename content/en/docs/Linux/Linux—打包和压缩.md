---
title: Linux 中的打包和压缩
date: 2021-04-30
author: LM
---

## 1.打包和压缩

在 Linux 中打包和压缩是两个概念，这源于 Linux 中很多压缩程序只能针对一个文件进行压缩，这样当你想要压缩一大堆文件时，你得先将这一大堆文件先打成一个包，然后再用压缩程序进行压缩。

- **打包是指将一大堆文件或目录变成一个总的文件**
- **压缩则是将一个大的文件通过一些压缩算法变成一个小文件**

## 2.常用的命令 tar

tar 命令可以为 linux 的文件和目录创建档案。利用 tar 命令，可以把一大堆的文件和目录全部打包成一个文件，这对于备份文件或将几个文件组合成为一个文件以便于网络传输是非常有用的。

```bash
# tar 
tar -xvf FileName.tar           # 解包
tar -cvf FileName.tar DirName   # 打包

# .tar.gz 和 .tgz 文件
tar -xzvf FileName.tar.gz          # 解压
openssl des3 -d -k password -salt -in FileName.tar.gz | tar xzf -       # 加密解压
tar -czvf - file | openssl des3 -salt -k password -out /path/to/file.tar.gz  # 加密压缩
tar -czvf FileName.tar.gz DirName  # 压缩

# .tar.bz2 和 .tar.xz 文件
tar -jxvf FileName.tar.bz2           # 解压
tar -jcvf FileName.tar.bz2 DirName   # 压缩

# .tar.bz 文件
tar -jxvf FileName.tar.bz    # 解压

# .tar.Z 文件
tar -Zxvf FileName.tar.Z            # 解压
tar -Zcvf FileName.tar.Z DirName    # 压缩

# 其他一些压缩文件格式
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
```

