---
title: git 出现 LF will be replaced by CRLF
date: 2021-01-15
author: LMingJian
---

## BUG 描述

在 Windows 平台下使用 git add，git deploy 提交文件时，经常出现`warning: LF will be replaced by CRLF`的提示。

## Resolution

这是因为在文本处理中，CR（CarriageReturn）/ LF（LineFeed）是不同操作系统上使用的换行符，当我们在 Windows 上的编辑器打开文件时，编辑器会把行尾的换行字符转换成回车和换行，或在用户按下 Enter 键时，插入回车和换行两个字符。

{{< details "回车（CR）和换行（LF）" >}}
回车符（CR）就是回到一行的开头，用符号 \ r 表示，十进制 ASCII 代码是 13，十六进制代码为 0x0D。
换行符（LF）就是另起一行，用 \n 符号表示，ASCII 代码是 10，十六制为 0x0A。
{{< /details >}}

在 Dos 和 Windows 平台，系统使用回车和换行两个字符来结束一行，即（ \r\n ），称为回车换行符。而在 Mac 和 Linux 平台，系统则只使用换行一个字符来结束一行，即( \n )。因此，为了保证代码在两个系统上保证一致，Git 需要对代码的换行符进行统一。

Git 可以在你提交时自动地把回车和换行转换成换行，而在检出（检查修改出入）代码时把换行转换成回车和换行。通过设置`core.autocrlf`属性，让 Git 自动处理转换。

```bash
# 提交时转换为 LF，检出时转换为 CRLF
git config --global core.autocrlf true
```

当然，你也可以仅针对检出进行转换，如下。

```bash
# 提交时转换为 LF，检出时不转换
git config --global core.autocrlf input
```

如果你是 Windows 程序员，且正在开发仅运行在 Windows 上的项目，可以设置 false 取消此功能，把回车保留在版本库中。

```bash
# 提交检出均不转换
git config --global core.autocrlf false
```

你也可以在文件提交时进行自检。

```bash
# 拒绝提交包含混合换行符的文件
git config --global core.safecrlf true   

# 允许提交包含混合换行符的文件
git config --global core.safecrlf false   

# 提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```

