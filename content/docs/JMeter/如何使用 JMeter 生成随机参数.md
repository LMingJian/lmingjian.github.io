---
title: 如何使用 JMeter 生成随机参数
date: 2024-12-25T13:54:27+08:00
author: LiangMingJian
---

# 在固定范围内随机取值

使用 **Tools - 函数助手**，选择函数`__Random`生成函数使用，例如：`${__Random(1, 5)}`，随机在 1-5 中取整数值。

# 随机从 CSV 文件中取值

使用 **Tools - 函数助手**，选择函数`__CSVRead`生成函数使用，例如：`${__CSVRead(example.csv, ${__Random(0, 5)})}`，随机从 CSV 文件的前 6 个数据中取值，注意其中 0 表示第一个值，5 表示第六个值。

{{< details "参考文件" >}} 
1：[ Jmeter随机参数各种搭配 ](https://www.cnblogs.com/wei9593/p/11944062.html)
{{< /details >}}
