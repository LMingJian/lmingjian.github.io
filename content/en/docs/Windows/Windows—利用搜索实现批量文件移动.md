---
title: Windows 系统利用搜索实现批量文件移动
date: 2021-01-13
author: LM
---

## 需求

如何将多个文件从一个文件夹移动至另一个文件夹中。

## 方法

通过 Windows 文件搜索实现批量文件的移动。

## 步骤

1. 找到目标文件夹
2. 查看文件夹中文件的格式，如`.jpg   .png   .txt`
3. 在确定需要移动的文件格式后，在右上角搜索框，输入格式如`.txt` 正则匹配文件名
4. 在出现的搜索结果中，按住`Ctrl+A`选择全部，按住`Ctrl+C`复制全部，然后，在其他地方新建文件夹，按住`Ctrl+V`复制所有选择的文件到新文件中即可

