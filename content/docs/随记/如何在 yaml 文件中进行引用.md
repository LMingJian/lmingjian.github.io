---
title: 如何在 yaml 文件中进行引用
date: 2024-12-25T10:32:51+08:00
author: LiangMingJian
---

# yaml 的锚点 & 与引用 *

**yaml 支持使用锚点 `&` 与引用 `*` 来实现高级编程语言中变量的操作**。

```yaml
localhost: &local
	host: 127.0.0.1
	base: 0
user:
	<<: *local
	db: 8
book:
	<<: *local
	db: 9
```

上述的 yaml 数据中，`&` 表示将 `local` 作为 `localhost` 里面数据的别名，即创建一个变量 `local={host: 127.0.0.1, base: 0}`。

`<<: *local` 的含义则代表将 `local` 这个数据合并入当前数据，最终，上面的结果类似下述的 yaml 数据：

> `<<: *local` 中，`<<` 代表合并，`*` 代表引用。

```yaml
localhost:
  host: 127.0.0.1
  base: 0

user:
  host: 127.0.0.1  # 继承自 localhost
  base: 0         # 继承自 localhost
  db: 8

book:
  host: 127.0.0.1  # 继承自 localhost
  base: 0         # 继承自 localhost
  db: 9
```

当然，同样支持只对某一个值进行锚点。

比如下面的数据，`&host` 创建变量 `host=127.0.0.1`，然后通过 `*host` 使用值。

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

上述的结果类似于：

```yaml
localhost: 
	host: 127.0.0.1
user:
	host: 127.0.0.1
	db: 8	
book:
	host: 127.0.0.1
	db: 9
```
