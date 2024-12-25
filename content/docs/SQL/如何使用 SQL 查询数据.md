---
title: 如何使用 SQL 查询数据
date: 2024-12-25T16:14:04+08:00
author: LiangMingJian
---

# 1.基本查询

要查询数据库表的数据，我们使用如下的 SQL 语句：

```sql
SELECT * FROM <'表名'>;
```

使用`SELECT * FROM students`时，SELECT 是关键字，表示将要执行一个查询，* 表示所有列，FROM 表示将要从哪个表查询，该 SQL 将查询出 students 表的所有数据。注意：查询结果也是一个二维表，它包含列名和每一行的数据。

SELECT 语句其实并不要求一定要有 FROM 子句：

```sql
SELECT 100+200;
```

上述查询会直接计算出表达式的结果。虽然 SELECT 可以用作计算，但它并不是 SQL 的强项。此外，不带 FROM 子句的 SELECT 语句还可以用来判断当前到数据库的连接是否有效，许多检测工具会执行一条 `SELECT 1;` 来测试数据库连接。

# 2.条件查询

SELECT 语句可以通过 WHERE 条件来设定查询条件，查询结果是满足查询条件的记录。

例如，要指定条件分数在 80 分或以上的学生，写成 WHERE 条件就是：

```sql
SELECT * FROM students WHERE score >= 80;
SELECT * FROM students WHERE score BETWEEN 60 AND 90
```

条件表达式可以用`<条件1> AND <条件2>`表达满足条件 1 并且满足条件 2 。

例如，符合条件分数在 80 分或以上，并且还符合条件男生，把这两个条件写出来：

```sql
SELECT * FROM students WHERE score >= 80 AND gender = 'M';
```

第二种条件是`<条件1> OR <条件2>`，表示满足条件 1 或者满足条件 2 。

例如，把上述 AND 查询的两个条件改为 OR，查询结果就是分数在 80 分或以上或者男生，满足任意之一的条件即选出该记录：

```sql
SELECT * FROM students WHERE score >= 80 OR gender = 'M';
```

第三种条件是`NOT <条件>`，表示不符合该条件的记录。

例如，写一个不是 2 班的学生这个条件:

```sql
SELECT * FROM students WHERE NOT class_id = 2;
SELECT * FROM students WHERE class_id <> 2;
```

要组合三个或者更多的条件，就需要用小括号表示如何进行条件运算。

例如，编写分数在 80 以下或者 90 以上，并且是男生的条件：

```sql
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';
```

如果不加括号，条件运算按照 NOT、AND、OR 的优先级进行，即 NOT 优先级最高，其次是 AND，最后是 OR，加上括号可以改变优先级。

常用的条件表达式包括：

| 条件                   | 表达式举例1     | 表达式举例2      | 说明                                                  |
| :--------------------- | :-------------- | :--------------- | :---------------------------------------------------- |
| 使用 = 判断相等        | score = 80      | name = 'abc'     | 字符串需要用单引号括起来                              |
| 使用 > 判断大于        | score > 80      | name > 'abc'     | 字符串比较根据 ASCII 码，中文字符比较根据数据库设置   |
| 使用 >= 判断大于或相等 | score >= 80     | name >= 'abc'    |                                                       |
| 使用 < 判断小于        | score < 80      | name <= 'abc'    |                                                       |
| 使用 <= 判断小于或相等 | score <= 80     | name <= 'abc'    |                                                       |
| 使用 <> 判断不相等     | score <> 80     | name <> 'abc'    |                                                       |
| 使用 LIKE 判断相似     | name LIKE 'ab%' | name LIKE '%bc%' | % 表示任意字符，例如 'ab%' 将匹配 'ab'，'abc'，'abcd' |

# 3.投影查询

如果我们只希望返回某些列的数据，而不是所有列的数据，我们可以用`SELECT 列1, 列2, 列3 FROM ...`，来让结果集仅包含指定列，这种操作称为投影查询。

```sql
SELECT id, score, name FROM students;
```

使用`SELECT 列1, 列2, 列3 FROM ..`.时，还可以给每一列起个别名，这样，结果集的列名就可以与原表的列名不同。它的语法是`SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...`。

```sql
SELECT id, score points, name FROM students;
```

投影查询同样可以接 WHERE 条件，实现复杂的查询。

```sql
SELECT id, score points, name FROM students WHERE gender = 'M';
```

# 4.聚合查询

对于统计总数、平均数这类计算，SQL 提供了专门的聚合函数，使用聚合函数进行查询，就是聚合查询，它可以快速获得结果。

`COUNT(*)` 表示查询所有列的行数，要注意聚合的计算结果虽然是一个数字，但查询的结果仍然是一个二维表，只是这个二维表只有一行一列，并且列名是`COUNT(\*)` ，因此，在使用聚合查询时，我们应该给列名设置一个别名，便于处理结果。

```sql
SELECT COUNT(*) num FROM students;
// num 10
```

`COUNT(*)` 和 `COUNT (id)` 实际上是一样的效果。

另外注意，聚合查询同样可以使用 WHERE 条件，因此我们可以方便地统计出有多少男生、多少女生、多少 80 分以上的学生等。但如果聚合查询的 WHERE 条件没有匹配到任何行，`COUNT()` 会返回 0，而 `SUM()、AVG()、MAX()、MIN()` 则会返回 NULL。

```sql
SELECT COUNT(*) boys FROM students WHERE gender = 'M';
```

常见的聚合函数：

| 函数 | 说明                                   |
| :--- | :------------------------------------- |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值                     |
| MIN  | 计算某一列的最小值                     |

对于聚合查询，SQL 还提供了分组聚合的功能。

例如，执行以下这个查询，`COUNT()`的结果不再是 1 个，而是 3 个，这是因为，GROUP BY 子句指定了按 class_id 分组，因此，执行该 SELECT 语句时，会把class_id 相同的列先分组，再分别计算，最终得到 3 行结果。

```sql
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;
```

# 5.多表查询

SELECT 查询不但可以从一张表查询数据，还可以从多张表同时查询数据。查询多张表的语法是：

```sql
SELECT * FROM <'表1'> <'表2'>
SELECT * FROM students, classes;
```

这种一次查询两个表的数据，查询的结果也是一个二维表，它是 students 表和 classes 表的乘积，即 students 表的每一行与 classes 表的每一行都两两拼在一起返回。结果集的列数是 students 表和 classes 表的列数之和，行数是 students 表和 classes 表的行数之积。

这种多表查询又称笛卡尔查询，使用笛卡尔查询时要非常小心，由于结果集是目标表的行数乘积，对两个各自有 100 行记录的表进行笛卡尔查询将返回 1 万条记录，对两个各自有 1 万行记录的表进行笛卡尔查询将返回 1 亿条记录。

此外，多表查询时，需要使用【表名.列名】这样的方式来引用列和设置别名，以避免结果集列名的重复。

```sql
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c
WHERE s.gender = 'M' AND c.id = 1;
```

# 6.连接查询

连接查询是另一种类型的多表查询。连接查询对多个表进行 JOIN 运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地连接在主表结果集上。例如，我们想要选出 students 表的所有学生信息，可以用一条简单的 SELECT 语句完成（从主表提取元素组成新表）。

```sql
SELECT s.id, s.name, s.class_id, s.gender, s.score FROM students s;
```

但是，假设我们希望结果集同时包含所在班级的名称，上面的结果集只有 class_id 列，缺少对应班级的 name 列。然而，存放班级名称的 name 列存储在 classes 表中。只有根据 students 表的 class_id，找到 classes 表对应的行，再取出 name 列，才能获得班级名称。

这时，连接查询就派上了用场。我们先使用最常用的一种内连接——INNER JOIN 来实现：

```sql
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id;
```

可以看到上面的语句中，首先使用 FROM <表1> 的语法确定主表 students s，再使用 INNER JOIN <表2> 的语法确定要连接的表 classes c，然后再使用 ON <条件> 来规定连接条件，这里的条件是 s.class_id = c.id，表示 students 表的 class_id 列与 classes 表的 id 列相同的行需要连接，最终输出结果。

常见的连接方式：

- INNER JOIN，只返回同时存在于两张表的行数据，由于 students 表的 class_id 包含 1，2，3，classes 表的 id 包含 1，2，3，4，所以，INNER JOIN 根据条件 s.class_id = c.id 返回的结果集仅包含 1，2，3。
- RIGHT OUTER JOIN，返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以 NULL 填充剩下的字段。
- LEFT OUTER JOIN，则返回左表都存在的行。如果我们给 students 表增加一行，并添加 class_id=5，由于 classes 表并不存在 id=5 的行，所以，LEFT OUTER JOIN 的结果会增加一行，对应的 class_name 是 NULL。
- FULL OUTER JOIN，它会把两张表的所有记录全部选择出来，并且，自动把对方不存在的列填充为 NULL。

{{< details "参考文件" >}} 
1：[ SQL教程 @廖雪峰 ](https://www.liaoxuefeng.com/wiki/1177760294764384)
{{< /details >}}
