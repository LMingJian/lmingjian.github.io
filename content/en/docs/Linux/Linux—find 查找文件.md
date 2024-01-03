---
title: Linux find 查找文件
date: 2023-11-16
author: LM
---

## 1.find 命令

基本格式：`find path expression`

### 1.1 按照文件名查找

```bash
find / -name httpd.conf　　# 在根目录下查找文件 httpd.conf
find /etc -name httpd.conf　　# 在/etc目录下文件httpd.conf
find /etc -name '*srm*'　　# 使用通配符 *，在/etc目录下查找文件名中含有字符串‘srm’的文件
```

### 2.1 按照文件特征查找 　　　　

```bash
find / -amin -10 　　# 查找在系统中最后10分钟访问的文件(access time)
find / -atime -2　　 # 查找在系统中最后48小时访问的文件
find / -empty 　　   # 查找在系统中为空的文件或者文件夹
find / -group cat 　 # 查找在系统中属于group为cat的文件
find / -mmin -5 　　 # 查找在系统中最后5分钟里修改过的文件(modify time)
find / -mtime -1 　　# 查找在系统中最后24小时里修改过的文件
find / -user fred 　 # 查找在系统中属于fred这个用户的文件
find / -size +1G　　# 查找出大于1G的文件(c:字节，w:双字，k:KB，M:MB，G:GB)
find / -size -1000k 　　# 查找出小于1000KB的文件
```

### 2.3 混合查找

```bash
find /tmp -size +10000c -and -mtime +2 　　# 在/tmp目录下查找大于10000字节并在最后2分钟内修改的文件
find / -user fred -or -user george 　　    # 在/目录下查找用户是fred或者george的文件文件
find /tmp ! -user panda　　                # 在/tmp目录中查找所有不属于panda用户的文件
```

## 2.grep 命令

基本格式：`grep expression file`

```bash
grep 'test' d*　　       # 显示所有以d开头的文件中包含test的行
grep 'test' aa bb cc 　　# 显示在aa，bb，cc文件中包含test的行
grep magic /usr/src　　  # 显示/usr/src目录下的文件(不含子目录)包含magic的行
grep -r magic /usr/src　 # 显示/usr/src目录下的文件(包含子目录)包含magic的行
```

