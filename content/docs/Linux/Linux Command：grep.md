---
title: Linux Command：grep
date: 2024-12-25T13:57:51+08:00
author: LiangMingJian
---

# 概述

grep ( global regular expression ) 命令用于查找文件里符合条件的字符串或正则表达式。

grep 指令用于查找内容包含指定的范本样式的文件，如果发现某文件的内容符合所指定的范本样式，预设 grep 指令会把含有范本样式的那一列显示出来。若不指定任何文件名称，或是所给予的文件名为 -，则 grep 指令会从标准输入设备读取数据。

```
grep [options] pattern [files]
```

# 示例

```
grep hello file.txt    # 在文件 file.txt 中查找字符串 "hello"，并打印匹配的行
echo "hello world" | grep world   # 在标准输入中查找字符串 "world"，并打印匹配的行
ifcpnfig | grep -C 2 eth0   # 在标准输出中查找字符串 "eth0"，并打印匹配的行以及前后 2 行的内容
```
