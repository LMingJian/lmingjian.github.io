---
title: Linux Command：ln
date: 2024-12-25T13:58:00+08:00
author: LiangMingJian
---

# 概述

`ln`（ link files ）命令可以为某一个文件在另外一个位置建立一个同步的链接。

当我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要的目录下都放一个必须相同的文件，我们只要在某个固定的目录，放上该文件，然后在其它的目录下用 `ln` 命令链接它就可以，不必重复的占用磁盘空间。

```
ln [参数] [源文件或目录] [目标文件或目录]
```

# 支持的参数

```
--backup[=CONTROL] 备份已存在的目标文件
-b 类似 --backup，但不接受参数
-d 允许超级用户制作目录的硬链接
-f 强制执行
-i 交互模式，文件存在则提示用户是否覆盖
-n 把符号链接视为一般目录
-s 软链接(符号链接)
-v 显示详细的处理过程
```

# 示例

```
ln -s log2013.log link2013 
# 将 log2013.log 链接到 link2013，输入 link2013 可访问 log2013.log
```