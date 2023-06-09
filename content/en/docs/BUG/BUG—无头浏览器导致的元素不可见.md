---
title: 无头浏览器导致的元素不可见
date: 2020-11-25
author: LMingJian
---

## BUG 描述

WebDriver 在使用 headless 时，默认会将窗口设置为 0x0，并且处于 Minimized 状态。这样会导致程序启动后，部分元素会因为超出界面而不可见，进而导致出现无法点击的异常。

## Resolution

在启动 WebDriver 时先配置浏览器大小，或在启动浏览器后执行最大化方法。

```python
options.add_argument('window-size=1920x1080')
options.add_argument('--start-maximized')
# 或者
browser.maximize_window()
```

