---
title: Linux 的常用命令
date: 2021-05-16
author: LM
---

## 1.Linux 的可执行命令

Linux 所有可执行命令都可以在文件目录`/bin`中进行查找。 

## 2.文件查看命令

```bash
ls  # 查看当前全部文件
ll  # 以详细信息列出文件
#------------------------------------------------
-a # 显示所有文件及目录 (. 开头的隐藏文件也会列出)
-l # 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
-r # 将文件以相反次序显示(原定依英文字母次序)
-t # 将文件依建立时间之先后次序列出
-A # 同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)
-F # 在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"
-R # 若目录下有文件，则以下之文件亦皆依序列出
```

## 3.文件夹操作命令

```bash
pwd    # 显示当前所在工作目录的全路径
cd ..  # 进入父文件夹
cd ~   # root
cd /   # 根
rm -rf xxxx  # 删除
vi test.txt  # 编辑
vim test.txt  # 编辑
chmod 755 filename # root 权限
chmod -R 755 filedirname # 对文件夹全部文件加权限
chown root KI.txt # 把所有者设置 root
less as.sh # 文件查看，q 退出，wq! 保存
head -n 5 as.sh # 查看文件的开头部分的内容，参数 -n 用于显示行数，默认为 10
tail as.sh # 查看文件
tail -f as.sh # 不断更新最后一行，循环查看
```

## 4.链接

```bash
ln -s log2013.log link2013 
# 将 log2013.log 链接到 link2013,输入 link2013 可访问 log2013.log
```

## 5.重定向

```bash
ping baidu.com >> A.txt
# 将命令的输出重定向到文档，即将结果写入文档，>不追加，>>追加
```

## 6.管道与搜索

```bash
echo ABCDE | grep A
# 管道命令（|）将前一个命令的输出作为后一个命令的输入，搜索命令（grep）对输入进行搜索
```

## 7.历史记录命令

```bash
history -c 
# 参数!n（n命令编号），执行第n个命令
# 参数!$ / !!（上一个命令），执行上一条命令，
# 参数-c（清理），清理历史记录
```

## 8.关机

```bash
shutdown
shutdown now
shutdown 13:20  
shutdown -p now  ### 关闭机器
shutdown -H now  ### 停止机器      
shutdown -r 09:35 ### 在 09:35am 重启机器
```

## 9.重启

```bash
reboot           ### 重启机器
reboot --halt    ### 停止机器
reboot -p        ### 关闭机器
```

