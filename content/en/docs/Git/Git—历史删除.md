---
title: 删除 Git 的提交历史
date: 2022-05-18
author: LM
---

## 1.删除特定文件的上传历史

通过重写提交记录，可以将过去提交的某些文件从整个仓库中移除。

```bash
# 重写提交记录，将历史中的所有指定文件删除
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch 目标的文件' --prune-empty --tag-name-filter cat -- --all
git filter-branch --force --index-filter 'git rm --cached -r --ignore-unmatch 目标的文件夹' --prune-empty --tag-name-filter cat -- --all

# 删除历史记录
rm -rf .git/refs/original/ 

# 将历史记录的过期时间设置为此刻，放弃所有历史的找回功能
git reflog expire --expire=now --all

# 垃圾回收
git gc --aggressive --prune=now 

# 强制更新（需要注意的是，远端仓库需要移除保护限制）
git push origin --force --all
```

