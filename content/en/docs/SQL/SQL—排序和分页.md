---
title: SQL 排序和分页
date: 2023-03-16
author: LM
---

## 1.排序

我们使用 ORDER BY 语句进行排序。

```sql
SELECT id, name, gender, score FROM students ORDER BY score;
```

我们可以加上 DESC 表示倒序。

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC;
```

如果 score 列有相同的数据，要进一步排序，可以继续添加列名。

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
```

上述表示结果先按 score 列倒序，如果有相同分数的，再按 gender 列排序。

默认的排序规则是 ASC-升序，即从小到大。ASC 可以省略，即`ORDER BY score ASC`和`ORDER BY score`效果一样。

如果有 WHERE 子句，那么 ORDER BY 子句要放到 WHERE 子句后面。例如，查询一班的学生成绩，并按照倒序排序：

```sql
SELECT id, name, gender, score
FROM students
WHERE class_id = 1
ORDER BY score DESC;
```

## 2.分页

分页实际上就是从结果集中截取出第 M~N 条记录。这个查询可以通过 LIMIT M OFFSET N 子句实现。

```sql
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;
```

上述查询 LIMIT 3 OFFSET 0 表示，对结果集从 0 号记录开始，最多取 3 条。注意 SQL 记录集的索引从 0 开始。

如果要查询第 2 页，那么我们只需要跳过头 3 条记录，也就是对结果集从 3 号记录开始查询，把 OFFSET 设定为 3。

```sql
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 3;
```

在分页时，需要注意以下问题：

- OFFSET 超过了查询的最大数量并不会报错，而是得到一个空的结果集。
- OFFSET 是可选的，如果只写 LIMIT 15，那么相当于 LIMIT 15 OFFSET 0。
- 在 MySQL 中，`LIMIT 15 OFFSET 30`还可以简写成`LIMIT 30, 15`。
- 使用 LIMIT M OFFSET N 分页时，随着 N 越来越大，查询效率也会越来越低。

{{< details "参考文件" >}} 
1：[ SQL教程 @廖雪峰 ](https://www.liaoxuefeng.com/wiki/1177760294764384)
{{< /details >}}