---
title: 如何在 yaml 文件中进行引用
date: 2022-01-22
author: LM
---

## 1.yaml 的锚点 & 与引用 *

yaml 支持使用锚点 `&` 与引用 `*` 来实现高级编程语言中变量的操作。

如下面的 yaml 数据，`&`表示将`localhost1`作为`localhost`的别名，`*`表示引用，`<<`表示将`localhost1`代表的`map`合并入当前`map`数据。

```yaml
localhost: &localhost1
	host: 127.0.0.1
user:
	<<: *localhost1
	db: 8
book:
	<<: *localhost1
	db: 9
```

同样的，也支持对某一个值进行锚点。如下面的数据，`&host`表示将`host`作为`host`这个值的别名，在使用`*host`时，将`host`这个值赋给目标。

```yaml
localhost: 
	host: &host 127.0.0.1
user:
	host: *host
	db: 8	
book:
	host: *host
	db: 9
```

