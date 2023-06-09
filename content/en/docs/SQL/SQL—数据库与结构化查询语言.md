---
title: SQL 结构化查询语言
date: 2020-09-27
author: LM
---

## 1.什么是 SQL(Structured Query Language)

SQL 是结构化查询语言的缩写，用来访问和操作数据库系统。SQL 语句既可以查询数据库中的数据，也可以添加、更新和删除数据库中的数据，还可以对数据库进行管理和维护操作。不同的数据库，都支持 SQL，这样，我们通过学习 SQL 这一种语言，就可以操作各种不同的数据库。

虽然 SQL 已经被 ANSI 组织定义为标准，不幸地是，各个不同的数据库对标准的 SQL 支持不太一致。并且，大部分数据库都在标准的 SQL 上做了扩展。也就是说，如果只使用标准 SQL，理论上所有数据库都可以支持，但如果使用某个特定数据库的扩展 SQL，换一个数据库就不能执行了。例如，Oracle 把自己扩展的SQL 称为 PL/SQL，Microsoft 把自己扩展的 SQL 称为 T-SQL。

现实情况是，如果我们只使用标准 SQL 的核心功能，那么所有数据库通常都可以执行。不常用的 SQL 功能，不同的数据库支持的程度都不一样。而各个数据库支持的各自扩展的功能，通常我们把它们称之为方言。

总的来说，SQL 语言定义了这么几种操作数据库的能力：

- DDL(Data Definition Language)：DDL 允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。
- DML(Data Manipulation Language)：DML 为用户提供添加、删除、更新数据的能力.
- DQL(Data Query Language)：DQL 允许用户查询数据，这也是通常最频繁的数据库日常操作。

PS: SQL 语言关键字不区分大小写！但是，针对不同的数据库，对于表名和列名，有的数据库区分大小写，有的数据库不区分大小写

## 2.什么是数据库

因为应用程序需要保存用户的数据，比如 Word 需要把用户文档保存起来，以便下次继续编辑或者拷贝到另一台电脑。要保存用户的数据，一个最简单的方法是把用户数据写入文件。例如，要保存一个班级所有学生的信息，可以向文件中写入一个 CSV 文件；如果要保存学校所有班级的信息，可以写入另一个 CSV 文件。

但是，随着应用程序的功能越来越复杂，数据量越来越大，如何管理这些数据就成了大问题

- 读写文件并解析出数据需要大量重复代码；
- 从成千上万的数据中快速查询出指定数据需要复杂的逻辑；

如果每个应用程序都各自写自己的读写数据的代码，一方面效率低，容易出错，另一方面，每个应用程序访问数据的接口都不相同，数据难以复用。

所以，数据库作为一种专门管理数据的软件就出现了。应用程序不需要自己管理数据，而是通过数据库软件提供的接口来读写数据。至于数据本身如何存储到文件，那是数据库软件的事情，应用程序自己并不关心，这样一来，编写应用程序的时候，数据读写的功能就被大大地简化了。

## 3.数据库的模型

数据库按照数据结构来组织、存储和管理数据，实际上，数据库一共有三种模型：

- 层次模型
- 网状模型
- 关系模型

层次模型就是以上下级的层次关系来组织数据的一种方式，层次模型的数据结构看起来就像一颗树：

```
            ┌─────┐
            │     │
            └─────┘
               │
       ┌───────┴───────┐
       │               │
    ┌─────┐         ┌─────┐
    │     │         │     │
    └─────┘         └─────┘
       │               │
   ┌───┴───┐       ┌───┴───┐
   │       │       │       │
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│     │ │     │ │     │ │     │
└─────┘ └─────┘ └─────┘ └─────┘
```

网状模型把每个数据节点和其他很多节点都连接起来，它的数据结构看起来就像很多城市之间的路网：

```
     ┌─────┐      ┌─────┐
   ┌─│     │──────│     │──┐
   │ └─────┘      └─────┘  │
   │    │            │     │
   │    └──────┬─────┘     │
   │           │           │
┌─────┐     ┌─────┐     ┌─────┐
│     │─────│     │─────│     │
└─────┘     └─────┘     └─────┘
   │           │           │
   │     ┌─────┴─────┐     │
   │     │           │     │
   │  ┌─────┐     ┌─────┐  │
   └──│     │─────│     │──┘
      └─────┘     └─────┘
```

关系模型把数据看作是一个二维表格，任何数据都可以通过行号+列号来唯一确定，它的数据模型看起来就是一个 Excel 表：

```
┌─────┬─────┬─────┬─────┬─────┐
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
├─────┼─────┼─────┼─────┼─────┤
│     │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘
```

随着时间的推移和市场竞争，最终，基于关系模型的关系数据库获得了绝对市场份额。目前，主流的关系数据库主要分为以下几类：

- 商用数据库，例如：Oracle，SQL Server，DB2 等
- 开源数据库，例如：MySQL，PostgreSQL 等
- 桌面数据库，以微软 Access 为代表，适合桌面应用程序使用
- 嵌入式数据库，以 Sqlite 为代表，适合手机应用和桌面程序

## 4.数据类型

对于一个关系表，除了定义每一列的名称外，还需要定义每一列的数据类型。关系数据库支持的标准数据类型包括数值、字符串、时间等。

## 5.关系模型

关系数据库是建立在关系模型上的。而关系模型本质上就是若干个存储数据的二维表，可以把它们看作很多 Excel 表。

- 表的每一行称为记录（Record），记录是一个逻辑意义上的数据。
- 表的每一列称为字段（Column），同一个表的每一行记录都拥有相同的若干字段。

字段定义了数据类型（整型、浮点型、字符串、日期等），以及是否允许为 NULL。注意 NULL 表示字段数据不存在。一个整型字段如果为 NULL 不表示它的值为 0，同样的，一个字符串型字段为 NULL 也不表示它的值为空串。通常情况下，字段应该避免允许为 NULL。不允许为 NULL 可以简化查询条件，加快查询速度，也利于应用程序读取数据后无需判断是否为 NULL。

关系数据库的表和表之间需要建立一对多，多对一和一对一的关系，这样才能够按照应用程序的逻辑来组织和存储数据。例如：

- 一个班级表，每一行对应着一个班级，而一个班级对应着多个学生，班级表和学生表的关系就是一对多。
- 反过来，我们先在学生表中定位了一行记录，要确定他的班级，学生表和班级表是多对一的关系。
- 如果我们把班级表分拆得细一点，单独创建一个教师表，班级表只存储教师ID，这样，一个班级总是对应一个教师，班级表和教师表就是一对一关系。

{{< details "参考文件" >}} 
1：[ SQL教程 @廖雪峰 ](https://www.liaoxuefeng.com/wiki/1177760294764384)
{{< /details >}}