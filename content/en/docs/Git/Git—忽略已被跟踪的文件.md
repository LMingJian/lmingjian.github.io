---
title: 忽略已被跟踪的文件
date: 2023-09-05
author: LM
---

## 1.忽略已被 Git 跟踪的文件

在初始化 git 仓库的时候，如果没有创建`.gitignore`文件来过滤不必要提交的文件，后来却发现某些文件不需要提交，但是这些文件已经被提交了，这时候创建`.gitignore`文件忽略这些文件时，发现ignore的规则对那些已经被 track 的文件无效。

实际上`.gitignore`文件只会忽略那些没有被跟踪的文件，也就是说 ignore 规则只对那些在规则建立之后被新创建的新文件生效。那么如何使`.gitignore`文件的规则对于那些已经被 track 的文件生效呢？

### a.删除 track 的文件 (已经  commit 的文件)

- `git rm 要忽略的文件` (如果目录下全是不需要的文件,使用 git rm -r --cached 命令)
- `git commit -a -m "删除不需要的文件"`

### b.在`.gitignore`文件中添加忽略规则

- 在`.gitignore`文件中添加ignore条目, 如: `some/path/some-file.ext`
- 提交`.gitignore`文件: `git commit -a -m "添加ignore规则"`

### c.推送到远程仓库 git push

{{< details "参考文件" >}} 
1：[ git 如何忽略已经提交的文件 @知乎 ](https://zhuanlan.zhihu.com/p/621054580)
{{< /details >}}