---
title: 如何使用 SQL 修改数据
date: 2024-12-25T16:14:08+08:00
author: LiangMingJian
---

# 增删查改

关系数据库的基本操作就是增删查改，即 CRUD：Create、Retrieve、Update、Delete。其中，对于查询，我们已经详细讲述了 SELECT 语句的详细用法。

而对于增、删、改，对应的 SQL 语句分别是：

- INSERT：插入新记录
- UPDATE：更新已有记录
- DELETE：删除已有记录

# 插入 INSERT

INSERT 语句的基本语法是：

```sql
INSERT INTO <'表名'> ('字段1', '字段2', '...') VALUES ('值1', '值2', '...');
INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛', 'M', 80);
```

注意到我们并没有列出 ID 字段，也没有列出 ID 字段对应的值，这是因为 ID 字段是一个自增主键，它的值可以由数据库自己推算出来。此外，如果一个字段有默认值，那么在 INSERT 语句中也可以不出现。

要注意，字段顺序不必和数据库表的字段顺序一致，但值的顺序必须和字段顺序一致。也就是说，可以写`INSERT INTO students (score, gender, name, class_id)`，但是对应的 VALUES 就得变成`(80, 'M', '大牛', 2)`。

还可以一次性添加多条记录，只需要在 VALUES 子句中指定多个记录值，每个记录是由 (...) 包含的一组值：

```sql
INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);
```

# 更新 UPDATE

UPDATE 语句的基本语法是：

```sql
UPDATE <'表名'> SET '字段1'='值1', '字段2'='值2', ... WHERE ...;
UPDATE students SET name='大牛', score=66 WHERE id=1;
```

注意到 UPDATE 语句的 WHERE 条件和 SELECT 语句的 WHERE 条件其实是一样的，因此完全可以一次更新多条记录：

```sql
UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
```

在 UPDATE 语句中，更新字段时可以使用表达式。例如，把所有 80 分以下的同学的成绩加 10 分：

```sql
UPDATE students SET score=score+10 WHERE score<80;
```

其中，`SET score=score+10`就是给当前行的 score 字段的值加上了 10。如果 WHERE 条件没有匹配到任何记录，UPDATE 语句不会报错，也不会有任何记录被更新。当没有 WHERE 条件时，整个表的所有记录都会被更新

# 删除 DELETE

DELETE 语句的基本语法是：

```sql
DELETE FROM <'表名'> WHERE ...;
DELETE FROM students WHERE id=1;
```

注意到 DELETE 语句的 WHERE 条件也是用来筛选需要删除的行，因此和 UPDATE 类似，DELETE 语句也可以一次删除多条记录

```sql
DELETE FROM students WHERE id>=5 AND id<=7;
```

同样，要特别小心的是，和 UPDATE 类似，不带 WHERE 条件的 DELETE 语句会删除整个表的数据

```sql
DELETE FROM students;
```

{{< details "参考文件" >}} 
1：[ SQL教程 @廖雪峰 ](https://www.liaoxuefeng.com/wiki/1177760294764384)
{{< /details >}}
