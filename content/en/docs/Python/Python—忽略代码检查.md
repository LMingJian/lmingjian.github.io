---
title: Python 代码如何忽略检查
date: 2021-11-18
author: LM
---

## 1.需求

在编码时，编辑器 IDE 往往会对代码质量进行检查，但有时候我不希望 IDE 对部分代码进行质量检查。

## 2.实现：#noqa

通过将 `#noqa` 添加到代码，来告知 IDE 的 linter 代码质量检查程序不要检查此行，该代码可能生成的任何警告都将被忽略。

```python
import Nothing  #noqa
```

## 3.实现：# noinspection XXX

通过 `# noinspection xxx` 来禁止特定检查或完全禁用给定范围的检查。可以使用 `# noinspection All` 来完全禁用检查。

```python
def test():
    # noinspection PyBroadException
    try:
        return sum(1+2)
    except Exception:
        return 0
```

PS：支持的检查 [Disabling and enabling inspections | PyCharm Documentation (jetbrains.com)](https://www.jetbrains.com/help/pycharm/disabling-and-enabling-inspections.html#comments-ref)