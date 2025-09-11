---
title: Python Package：time
date: 2025-09-08T19:42:22+08:00
author: LiangMingJian
---

# 概述

> 标准库

time 是 Python 中提供时间访问和转换的一个模块，通过该模块可以完成诸如时间戳转换，时间信息格式化等功能。

此外，在 Python 中，操作时间的模块除了 time 模块外，还有 datetime（更多用于年月日，时分秒处理）这个模块供开发者使用。

datetime 模块的使用可查阅另外一篇文章：**Python Package：datetime**

# 获取当前时间戳

time 模块提供方法 `time()` 用以返回自 1970 年 1 月 1 日（UNIX纪元）以来到现在的秒数，即当前的时间戳。

```python
import time

timestamp = time.time()
print(timestamp)
```

# 结构化时间戳

time 模块提供方法 `localtime()` 和 `gmtime()` 将给定时间戳转换为标准格式的 struct_time 对象（一个包含具名元组，支持点式属性调用的对象，支持的属性有 tm_year，tm_mon，tm_mday，tm_hour，tm_min，tm_sec 等）。

其中 `localtime()` 以本地时区格式进行转换，`gmtime()` 以 UTC （协调世界时）时区格式转换（时间戳 0 就是 UTC 1970-01-01）。

```python
import time  

# Asia/Shanghai 中国时区 2025-01-01
timestamp = 1735660800  

# 以本地系统时区转换（中国时区）
localtime = time.localtime(timestamp)  
print(localtime)  
# time.struct_time(tm_year=2025, tm_mon=1, tm_mday=1, 
#                  tm_hour=0, tm_min=0, tm_sec=0, 
#                  tm_wday=2, tm_yday=1, tm_isdst=0)

# 以 UTC 时区转换（比中国时间晚 8 小时）
gmtime = time.gmtime(timestamp)  
print(gmtime)
# time.struct_time(tm_year=2024, tm_mon=12, tm_mday=31, 
#                  tm_hour=16, tm_min=0, tm_sec=0, 
#                  tm_wday=1, tm_yday=366, tm_isdst=0)
```

# 格式化 struct_time 对象输出

struct_time 对象支持使用方法 `strftime()` 和 `asctime()` 进行格式化输出，将对象以字符串格式进行输出。

其中 `asctime()` 默认以 `Wed Jan  1 00:00:00 2025` 格式输出，`strftime()` 则支持以自定格式输出，常用格式如 `%Y-%m-%d %H:%M:%S`。

```python
import time  

# Asia/Shanghai 中国时区 2025-01-01
timestamp = 1735660800  
localtime = time.localtime(timestamp)  

# 以标准格式输出
print(time.asctime(localtime))  
# Wed Jan  1 00:00:00 2025

# 以自定义格式输出
print(time.strftime("%Y-%m-%d %H:%M:%S", localtime))
# 2025-01-01 00:00:00
```

> `strftime()` 常用格式有：%Y（年），%m（月），%d（日），%H（24 小时制），%I（12 小时制），%M（分钟），%S（秒），%f（微秒）

> `strftime()` 所有支持格式可查阅 [ time.strftime 文档 ](https://docs.python.org/zh-cn/3.13/library/time.html#time.strftime)

> `asctime()` 存在兄弟方法 `ctime()`。`ctime()` 当传入时间戳数据时，功能与 `asctime()` 一样，但置空不传入数据时，可直接返回格式化后的**本地当前时间**。

# 将字符串转换为 struct_time 对象

time 模块提供方法 `strptime()` 将符合一定格式的字符串转换回 struct_time 对象。匹配的格式支持应当与 `strftime()` 所支持的一致。

```python
import time  
  
data = time.strptime('2025-01-01 00:00:00', "%Y-%m-%d %H:%M:%S")  
print(data)
# time.struct_time(tm_year=2025, tm_mon=1, tm_mday=1, 
#                  tm_hour=0, tm_min=0, tm_sec=0, 
#                  tm_wday=2, tm_yday=1, tm_isdst=-1)
```

# 将 struct_time 对象转换回时间戳

time 模块提供方法 `mktime()` 将 struct_time 对象转换回时间戳，需要注意的是，这里的时间戳是本地时区，`mktime()` 是 `localtime()` 的反转方法。

```python
import time  

# Asia/Shanghai 中国时区 2025-01-01
timestamp = 1735660800  
localtime = time.localtime(timestamp)  

# 将 struct_time 对象转换回时间戳
mktime = time.mktime(localtime)  
print(mktime)
# 1735660800.0
```

# 休眠

time 模块提供 `sleep()` 方法用来暂停线程执行。

```python
import time

# 暂停执行 5 s
time.sleep(5)
```

# 其他功能

获取一个不受系统影响的单调时钟时间：`time.monotonic()`。

获取一个性能计数器时间（包括睡眠）：`time.perf_counter()`。

返回时区信息：`time.tzname`。

————————————

> [ time --- 时间的访问和转换 ](https://docs.python.org/zh-cn/3.13/library/time.html)
