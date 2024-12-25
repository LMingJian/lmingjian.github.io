---
title: 如何在 Selenium 中监听控制台报错
date: 2024-12-25T16:13:30+08:00
author: LiangMingJian
---

# 需求

在执行 Selenium 脚本时，需要监听控制台报错。

# 实现

```python
_options = EdgeOptions()
d = DesiredCapabilities.EDGE
d['loggingPrefs'] = {'browser': 'ALL'}
_driver_path = r'msedgedriver.exe'
_browser = Edge(service=Service(executable_path=_driver_path), options=_options, capabilities=d)
_browser.get('http://1.1.1.1')
sleep(5)
for entry in _browser.get_log('browser'):
    print(entry)
_browser.close()
```

# 支持监听的 Log 类型

```javascript
public class LogType{
    public static final String BROWSER = "browser";
    public static final String CLIENT = "client";
    public static final String DRIVER = "driver";
    public static final String PERFOMANCE = "performance";
    public static final String PROFILER = "profiler";
    public static final String SERVER = "server";
}
```
