---
title: Linux Command：grep
date: 2024-12-25T13:57:51+08:00
author: LiangMingJian
---

# 概述

grep ( global regular expression ) 命令用于查找文件里符合条件的字符串或正则表达式，如果程序发现某文件的内容符合所指定的范本样式，grep 会把含有范本样式的那一列显示出来。

```
grep [options] pattern [files]
```

# 示例

```shell
# 在文件 file.txt 中查找字符串 "hello"，并打印匹配的行
grep hello file.txt    

# 在标准输入中查找字符串 "world"，并打印匹配的行
echo "hello world" | grep world   

# 在标准输出中查找字符串 "eth0"，并打印匹配的行以及前后 2 行的内容
ifcpnfig | grep -C 2 eth0   
```
