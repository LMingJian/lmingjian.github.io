---
title: 修复 mysqldump 导出中文乱码的问题
date: 2025-03-01T11:17:48+08:00
author: LiangMingJian
---

# BUG 描述

在 windows powershell 中使用 `mysqldump -h 127.0.0.1 --default-character-set=utf8 -uroot -proot xxx > xxx.sql` 导出数据库时，导出的文件打开查看会看到中文乱码的问题，如果查看编码，会发现编码是 UTF16，而不是 UTF8。

# Resolution

这个问题的出现不是因为 mysqldump 的异常，而是 windows powershell 的问题。

Windows PowerShell 输出重定向命令 `>` 的文件编码默认是 UTF-16 (LE)。因此，如果我们使用上述命令备份数据库，会因为重定向，导致中文的编码异常。

> Note A dump made using PowerShell on Windows with output [redirection](https://zhida.zhihu.com/search?content_id=195143047&content_type=Article&match_order=1&q=redirection&zhida_source=entity) creates a file that has UTF-16 encoding:  
> 
> mysqldump [options] > dump.sql  
> 
> However, UTF-16 is not permitted as a connection character set (see Impermissible Client Character Sets), so the dump file cannot be loaded correctly. To work around this issue, use the --result-file option, which creates the output in ASCII format:  
> 
> mysqldump [options] --result-file=dump.sql

根据 Mysql 官网的上述解释，可以使用 `--result-file` 来指定导出文件，而不是使用输出重定向。

```
mysqldump -h 127.0.0.1 --default-character-set=utf8 --result-file=D:\xxx.sql -uroot -proot xxx
```
