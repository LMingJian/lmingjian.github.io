---
title: 如何使用 EXPLAIN 分析性能
date: 2025-06-27T16:07:24+08:00
author: LiangMingJian
---

# EXPLAIN

`EXPLAIN` 通过展示查询的**执行计划（Execution Plan）**，直观呈现以下内容：

- **执行步骤顺序**（`id`字段）
- **表访问方式**（`type`字段）
- **索引使用情况**（`key`字段）
- **数据扫描量预估**（`rows`字段）
- **额外操作开销**（`Extra`字段）

例如：

```sql
EXPLAIN SELECT * FROM orders WHERE user_id = 100;
```

输出结果类似：

```javascript
id | select_type | table  | type | possible_keys | key     | rows | Extra
1  | SIMPLE      | orders | ref  | idx_user      | idx_user| 15   | Using where
```

此结果说明：引擎通过索引 `idx_user` 快速定位到 15 行数据。

`type = ref` 表示使用了索引。

# type

- **ALL**：全表扫描（性能杀手⚠️）
- **index**：全索引扫描（需优化）
- **range**：索引范围扫描（较高效）
- **ref/eq_ref**：索引精确匹配（最优解）

# Extra

- **Using filesort**：内存外排序（消耗CPU）
- **Using temporary**：创建临时表（内存/磁盘开销）
- **Using index**：覆盖索引（理想状态）

{{< details "参考文件" >}} 
1：[ EXPLAIN工具：查询执行计划分析与索引诊断 ](https://bbs.huaweicloud.com/blogs/454482)
{{< /details >}}
