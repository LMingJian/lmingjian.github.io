---
title: .gitignore 文件配置
date: 2022-05-18
author: LM
---

## 1.文件语法

用户可以在存储库的根目录中创建 .gitignore 文件，指示 Git 在进行提交时要忽略哪些文件和目录。

### a.忽略指定文件/目录

```bash
# 忽略指定文件
HelloWrold.class

# 忽略指定文件夹
bin/
bin/gen/
```

### b.通配符忽略规则

```bash
# 忽略 .class 的所有文件
*.class

# 忽略名称中末尾为 ignore 的文件夹
*ignore/

# 忽略名称中间包含 ignore 的文件夹
*ignore*/
```

