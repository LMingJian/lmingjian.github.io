---
title: 如何构建 JMeter 测试计划
date: 2024-12-25T13:54:13+08:00
author: LiangMingJian
---

# 概述

‎在下文中，将介绍如何创建一个基本‎‎的测试计划‎‎来测试 Web 网站。您将创建五个用户，将请求发送到 JMeter 网站上的两个页面。此外，您还会告诉用户运行他们的测试两次。因此，请求总数为 （5 个用户） x （2 个请求） x （重复 2 次） = 20 个 HTTP 请求。

# 设定测试线程数量

要执行测试计划，您要做的第一步就是添加‎‎线程组‎‎元素。线程组会告诉 JMeter 您要模拟的用户数量、用户发送请求的频率以及他们应发送的请求数量。‎

首先选择测试计划，然后单击右键获取添加菜单，然后选择添加—>线程组。‎

![](/_images/drawingbed/img/202204291754285.png)

然后为我们的线程组提供更具描述性的名称。在名称字段中，输入 JMeter 用户。‎接下来，将用户数（称为线程）增加到 5。‎‎在下一个字段中，延时保留默认值 1 秒，此属性告诉 JMeter 在启动每个用户之间延迟多长时间。例如，如果您输入了 5 秒的 Ramp-Up ，JMeter 将在 5 秒结束时完成启动所有用户，因此，如果我们有 5 个用户和 5 秒的 Ramp-Up ，则启动用户之间的延迟将是 1 秒（5 个用户 / 5 秒 = 每秒 1 个用户）。如果将值设置为 0，则 JMeter 将立即启动所有用户。‎

‎最后在循环计数字段中输入 2 值。此属性告诉 JMeter 重复测试的次数。如果您输入 1 的循环计数值，则 JMeter 将只运行您的测试一次。要让 JMeter 反复运行您的测试计划，请选择 Infinite。

![](/_images/drawingbed/img/202204291754077.png)

# 添加 HTTP 请求默认值

右键获取 Add 菜单，选择 Add → Config Element → HTTP Request Defaults。在 Path 中填入默认路径

![](/_images/drawingbed/img/202204291754950.png)

# 添加 Cookie 管理器

几乎所有的 Web 测试都应使用 Cookie 支持，除非您的应用程序特别不使用 Cookie。要添加 Cookie 支持，只需在测试计划中向每个线程组添加 **HTTP Cookie Manager** 即可。

![](/_images/drawingbed/img/202204291754742.png)

# 添加 HTTP 请求

右键，添加→采样器→ HTTP 请求，添加以下两个请求

![](/_images/drawingbed/img/202204291754840.png)

![](/_images/drawingbed/img/202204291755738.png)

# 添加结果监听器

监听器负责将 HTTP 请求的所有结果存储在文件中，并呈现数据的可视化模型。‎这里我们选择 Graph Results listener (Add → Listener → Graph Results listener)

{{< details "参考文件" >}} 
1：[Apache JMeter - User's Manual: Building a Web Test Plan](https://jmeter.apache.org/usermanual/build-web-test-plan.html)
{{< /details >}}
