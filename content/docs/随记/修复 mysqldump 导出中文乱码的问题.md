---
title: 修复 mysqldump 导出中文乱码的问题
date: 2025-03-01T11:17:48+08:00
author: LiangMingJian
---

# BUG 描述

在 Windows 终端备份 Mysql 数据库时，使用到以下命令进行：

```shell
mysqldump -h 127.0.0.1 --default-character-set=utf8 -uroot -p123456 xxx > xxx.sql
```

在成功执行命令后，该导出的 sql 文件没办法执行，即无法重新导入会 Mysql 数据库。

此时，打开导出的文件，我们会看到中文乱码的问题。使用 VSCode 打开 sql 文件，可以查看到右下角的编码格式 UTF16，而不是设置的 UTF8。

# BUG 解决

这个问题 mysqldump 的错，而是 Windows powershell 的错。

Windows PowerShell 输出重定向命令 > 的文件编码默认是 UTF-16。

因此，即使我们在 mysqldump 命令中指定了 UTF-8 编码格式，但其作用范围也只去到该命令的执行完成。在重定向输出时，Windows PowerShell 会自动的将 mysqldump 命令的 UTF-8 结果转换成 UTF-16 格式输出，最终导致编码异常，乱码。

如何解决？

在官网文档中，Mysql 给出了解决方案，即通过 `--result-file` 指定输出文件，而不应该使用重定向输出。

```shell
mysqldump -h 127.0.0.1 --default-character-set=utf8 --result-file=D:\xxx.sql -uroot -proot xxx
```

![](_images/drawingbed/img/Pasted%20image%2020250916190642.png)

> [ mysqldump — A Database Backup Program ](https://dev.mysql.com/doc/refman/8.4/en/mysqldump.html)
