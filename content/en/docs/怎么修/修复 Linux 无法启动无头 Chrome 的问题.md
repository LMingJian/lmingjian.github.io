---
title: 修复 Linux 无法启动无头 Chrome 的问题
date: 2021-01-13
author: LMingJian
---

## BUG 描述

linux 系统下， Python 脚本无法使用 selenium 启动 Chrome，报错：`unknown error: DevToolsActivePort file doesn't exist`。

## Resolution

Chrome 在 Linux 下权限不足，需要添加以下属性以 Root 运行。

```python
options.add_argument('--no-sandbox')  # 不在沙盒运行，以 Root 权限运行
```

