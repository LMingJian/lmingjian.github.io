---
title: 修复非法字符 ufeff 的问题
date: 2022-07-29
author: LMingJian
---

## BUG 描述

在使用 Python 读取 txt 文本时，出现错误：`\ufeff isn't UTF-8` ，无法识别字符 `\ufeff`。

## Resolution

这是因为文件编码使用了 UTF8+BOM 格式的原因，将文件重新保存为 UTF-8 即可。

{{< details "什么是 BOM?" >}}
BOM 的全称是 Byte Order Mark（定义字节顺序），是 Windows 的一种文本文件的编码方式。 Windows 通过使用 BOM 来告诉编辑器当前文件采用何种编码格式，便于编辑器识别。
{{< /details >}}

