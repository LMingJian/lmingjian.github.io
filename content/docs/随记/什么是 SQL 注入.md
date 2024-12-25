---
title: 什么是 SQL 注入
date: 2024-12-25T10:34:01+08:00
author: LiangMingJian
---

# 什么是 SQL 注入

SQL注入是一种非常常见的数据库攻击手段，也是网络世界中最普遍的漏洞之一。SQL注入通过在表单中填写包含SQL关键字的数据来使数据库执行非常规代码来完成数据库攻击。

# 示例

如下述查询语句，从 admin 表中查找 user 为 test 并且 pass 为 123456 的记录，并将满足要求的记录输出。

```sql
SELECT * FROM admin WHERE user = "test" AND pass = "123456";
```

此时，如果用户输入的密码是`" OR "1"="1` ，用户名是 test ，那么这个语句就会变为以下的样子。

```sql
SELECT * FROM admin WHERE user = "test" AND pass = "" OR "1"="1";
```

显然，WHERE 后的表达式一定为真，于是数据库会将每条记录都进行输出，而不仅输出 test 用户数据，数据库攻击就这样完成了。
