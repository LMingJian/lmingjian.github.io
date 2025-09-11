---
title: Python Package：datetime
date: 2025-09-09T11:13:45+08:00
author: LiangMingJian
---

# 概述

> 标准库

datetime 是 Python 中提供日期处理的一个模块，通过该模块可以完成诸如日期格式化，时间格式化，日期计算等功能。

datetime 模块的组成主要包括 datetime，date，time 这 3 个对象以及一个 timedelta 对象。

此外，在 Python 中，操作时间的模块除了 datetime 模块外，还有 time（更多用于时间戳处理）这个模块供开发者使用。

time 模块的使用可查阅另外一篇文章：**Python Package：time**

# datetime 对象

datetime 对象是一个包含年月日，时分秒信息的对象，支持通过点式访问包括 year，month，day，hour，minute，second，microsecond 在内的数据。

## 构造 datetime 对象

```python
from datetime import datetime

# 按顺序传入年月日，时分秒，（毫秒，时区）
date1 = datetime(2025, 12, 31, 10, 10, 10)  
print(date1)
# 2025-12-31 10:10:10
```

## 获取当前完整日期时间

通过类方法 `today()` 或 `now()` 获取当前的年月日，时分秒，毫秒信息。

其中 `now()` 支持携带时区参数 tz，`today()` 不支持

```python
from datetime import datetime
from datetime import timezone
  
# 获取当前时间  
date1 = datetime.now()  
date2 = datetime.today()  
  
print(date1)  # 2025-09-09 11:24:34.755896  
print(date1.microsecond)  # 755896  
print(date2)  # 2025-09-09 11:24:34.755896  
  
# 获取指定时区的当前时间  
date3 = datetime.now(tz=timezone.utc)  
print(date3)  # 2025-09-09 03:30:01.321534+00:00
```

## 从时间戳获取日期时间

通过类方法 `fromtimestamp()` 传入时间戳和时区数据可以将时间戳按指定的时区转换为标准的 datetime 日期对象。

特别的，当时区数据不传入时，默认以当前本地时区为格式转换。

```python
from datetime import datetime
from datetime import timezone 

# Asia/Shanghai 中国时区 2025-01-01
timestamp = 1735660800  

# 以本地系统时区转换（中国时区）
date1 = datetime.fromtimestamp(timestamp)  
print(date1)  
# 2025-01-01 00:00:00

# 以 UTC 时区转换（比中国时间晚 8 小时）
date2 = datetime.fromtimestamp(timestamp, tz=timezone.utc)
print(date2)
# 2024-12-31 16:00:00+00:00
```

## 将时间转换为时间戳

通过类方法 `timestamp()` 可以获取 datetime 对象的时间戳数据。

```python
from datetime import datetime 

date1 = datetime(2025, 1, 1, 0, 0, 0)  
print(date1.timestamp())
```

## 获取星期（周）数据

datetime 对象支持使用 `isocalendar()`（返回年，周次，周天），`isoweekday()`（周天，周一为 1），`weekday()`（周天，周一为 0）等方法返回周数据。

```python
from datetime import datetime

date1 = datetime(2025, 1, 1, 0, 0, 0)  
print(date1.isocalendar())  # (year=2025, week=1, weekday=3)
print(date1.isoweekday())  # 3
print(date1.weekday())    # 2
```

## 格式化输出和格式化转换

datetime 对象支持以字符串形式格式化输出，也支持从格式化的字符串中转换。

```python
from datetime import datetime

# 格式化输出
date1 = datetime.today()  
print(date1.strftime("%Y-%m-%d %H:%M:%S"))  
# 2025-09-09 11:44:25

# 格式化转换
date2 = datetime.strptime("2025-01-01 00:00:00", 
						  "%Y-%m-%d %H:%M:%S")  
print(date2)
# 2025-01-01 00:00:00
```

> `strftime()` 常用格式有：%Y（年），%m（月），%d（日），%H（24 小时制），%I（12 小时制），%M（分钟），%S（秒），%f（微秒）

> `strftime()` 所有支持格式可查阅 [ strftime 和 strptime 格式码文档 ](https://docs.python.org/zh-cn/3.13/library/datetime.html#strftime-and-strptime-format-codes)

# date 对象

date 对象是 datetime 对象中年月日部分的内容。

date 对象的使用与 datetime 对象基本一致，只是 date 对象存储的数据只有年月日，不包括时分秒，以及时区。

## 构造 date 对象

```python
from datetime import date

date1 = date(2025, 1, 1)  
print(date1)
# 2025-01-01
```

## 获取当前年月日

```python
from datetime import date  

# 获取当前年月日
date1 = date.today()  
print(date1)       # 2025-09-09
print(date1.year)  # 2025
```

## 从时间戳获取年月日

与 datetime 对象不同，date 的 `fromtimestamp()` 方法没有时区设置功能。

```python
from datetime import date  
  
# Asia/Shanghai 中国时区 2025-01-01
timestamp = 1735660800  
  
# 以本地系统时区转换（中国时区）(没有时区设置功能)  
date1 = date.fromtimestamp(timestamp)  
print(date1)  
# 2025-01-01
```

## 格式化输出

与 datetime 对象不同，date 只支持格式化输出 `strftime()` 方法。

```python
from datetime import date  
  
# 格式化输出  
date1 = date.today()  
print(date1.strftime("%Y-%m-%d"))  
# 2025-09-09
```

# time 对象

time 对象是 datetime 对象的时分秒部分，但与 date 对象不同，time 对象没有过多的功能，更多只是作为一个对象使用。

## 构造 time 对象

```python
from datetime import time  

# 构造时间 10:10:10
time1 = time(10,10,10)  
print(time1)
# 10:10:10
```

## 格式化输出

```python
from datetime import date  
  
# 格式化输出  
time1 = time(10,10,10)
print(time1.strftime("%H:%M:%S"))  
# 10:10:10
```

## 合并 time 对象和 date 对象

```python
from datetime import datetime, date, time  
  
date1 = date(2025, 10, 10)  
time1 = time(10, 10, 10)  

# 合并 time 对象与 date 对象
datetime1 = datetime.combine(date1, time1)  
print(datetime1)
# 2025-10-10 10:10:10
```

# timedelta 对象

在 datetime 模块中，最具特色的一点在于 datetime 对象和 date 对象是支持计算的。 datetime 对象和 date 对象计算的结果就是 timedelta 对象（datetime 对象与 date 对象支持的计算基本一致，下文以 datetime 对象为例）。

## 构造 timedelta 对象

`class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)`

timedelta 对象支持传入天，秒，毫秒，微秒，分，时，周进行构造，但只有天，秒，毫秒会被存储，其他传入数据会自动换算为毫秒，秒，天进行存储。

```python
from datetime import timedelta

delta = timedelta(
    days=50,
    seconds=27,
    microseconds=10,
    milliseconds=29000,
    minutes=5,
    hours=8,
    weeks=2
)
print(delta)
# datetime.timedelta(days=64, seconds=29156, microseconds=10)
```

## timedelta 对象的运算

timedelta 对象支持与 timedelta 对象计算，常用的运算有：

|运算|功能|
|---|---|
|`t1 = t2 + t3`|和运算|
|`t1 = t2 - t3`|差运算|
|`t1 = t2 * i`|乘运算|
|`t1 = t2 * f`|浮点乘运算，结果四舍五入|
|`f = t2 / t3`|除运算，结果是一个浮点数。|
|`t1 = t2 / f`|浮点除运算，结果四舍五入|

## 与 datetime 进行计算

通过构造 timedelta 对象可以与 datetime 进行相加相减计算，用以求指定日期后多少天，或前多少天。

datetime 对象支持的对象有：

|运算     |功能     |
|---|---|
|`timedelta = datetime1 - datetime2`|间隔计算|
|`datetime2 = datetime1 + timedelta`|间隔加|
|`datetime2 = datetime1 - timedelta`|间隔减|
|`datetime1 ==/!= datetime2`|相等比较|
|`datetime1 </<=/>/>= datetime2`|先后比较|

————————————

> [ datetime --- 基本日期和时间类型 ](https://docs.python.org/zh-cn/3.13/library/datetime.html)
