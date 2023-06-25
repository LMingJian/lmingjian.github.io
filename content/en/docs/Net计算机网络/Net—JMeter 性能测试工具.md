---
title: JMeter 性能测试工具
date: 2020-11-25
author: LM
---

## 1.Apache JMeter™

Apache JMeter™ 是一个开源的，纯 Java 编写的性能测试软件。JMeter 可以运行在任何具有合规 Java 的系统上（即有 Java 环境），下载最新的生产版本后，进入 bin 文件夹，运行  jmeter.bat （用于 Windows）或  jmeter（用于 Unix）文件启动 JMeter。

[ Apache JMeter 下载 ](http://jmeter.apache.org/download_jmeter.cgi)

## 2.如何开始

测试计划描述了 JMeter 在运行时将执行的一系列步骤。完整的测试计划将由一个或多个线程组、逻辑控制器、生成控制器、监听器、计时器、断言和配置元素（Thread Groups，logic controllers，sample generating controllers，listeners，timers，assertions  and  configuration elements）组成。

### 2.1 添加和删除元素

通过右键单击树中的元素，并从"**add**" 列表中选择新元素，就可以在测试计划中添加元素。

### 2.2 配置树元素

测试树中的任何元素都会在 JMeter 的右侧框架中呈现控件。这些控件允许您配置该特定测试元素的行为。元素的配置取决于元素的类型。测试树本身可以通过拖动和丢弃测试树周围的组件来操作。

### 2.3 保存测试计划

我们建议您在运行之前将测试计划保存到文件中。要保存测试计划，请从文件菜单中选择 "**Save**" 或 "**Save Test Plan As ...**"

JMeter 允许您保存整个测试计划树或仅保存其中的一部分。要仅保存位于测试计划树特定"分支"中的元素，请选择从该树开始"分支"的树中的测试计划元素，然后单击右鼠标按钮选择 "**Save Selection As …**" 或者从编辑菜单中选择  "**Save Selection As …**"。

### 2.4 运行测试计划

要运行您的测试计划，请从 "**Run**" 菜单项中选择 "**Start**"。当 JMeter 运行时，它会在菜单栏下方的部分右侧显示一个小的绿色框。绿色框左侧的数字是活动线程 / 线程总数。这些仅适用于本地运行测试；它们不包括在使用客户端服务器模式时在远程系统上启动的任何线程。

**仅在调试测试计划时才应使用此处描述的 GUI 模式。要运行真正的负载测试，请使用 CLI 模式。**

### 2.5 停止测试

菜单中提供两种类型的停止命令：

- **Stop** - 如果可能，立即停止线程。停止命令将检查所有线程是否在默认超时内停止，即 5000 ms = 5 秒，[ 这可以使用 JMeter 属性**jmeterengine.threadstop.wait** 更改 ]。如果线程未停止，则会提示信息。停止命令可以重试，但如果失败，则必须退出 JMeter 进行清理。
- **Shutdown** - 在当前工作结束时停止线程。终止命令不会中断任何活动示例。模式关闭对话框将保持活动状态，直到所有线程停止。

如果关闭时间过长。请关闭对话框并选择 **Run** / **Stop**

当在 CLI 模式下运行 JMeter 时，JMeter CLI 模式将监听特定端口上的命令（ 默认为**4445，**请参阅 JMeter 属性 **jmeterengine.nongui.port** ），所选端口会显示在控制台窗口中，目前支持的命令包括：

- **Shutdown** - 优雅的关闭
- **StopTestNow** - 立即关闭

这些命令可以通过 **shutdown[ .cmd | .sh ]** 或 **stoptest[ .cmd | .sh ]** 脚本发送。这些脚本将在 JMeter **Bin**目录中找到。

### 2.6错误报告

JMeter 向 **jmeter .log** 文件写入有关测试运行本身的一些信息报告警告和错误。

采样错误（例如 HTTP 404 - 未找到的文件）通常不在日志文件中报告。相反，这些存储为样本结果的属性。示例结果的状态可以在各种不同的监听器中看到。

## 3.线程组Thread Group

一个性能测试请求负载是基于一个线程组完成的。一个测试计划必须有一个线程组。在测试计划下面多个线程是并行执行的，也就是说这些线程组是同时被初始化并同时执行线程组下的 Sampler 的。

线程组主要包含三个参数：线程数、准备时长（Ramp-Up Period(in seconds)）、循环次数。

- 线程数：虚拟用户数。一个虚拟用户占用一个进程或线程。设置多少虚拟用户数在这里也就是设置多少个线程数。
- 准备时长：设置的虚拟用户数需要多长时间全部启动。如果线程数为20 ，准备时长为10 ，那么需要10秒钟启动20个线程。也就是每秒钟启动2个线程。
- 循环次数：每个线程发送请求的次数。如果线程数为20 ，循环次数为100 ，那么每个线程发送100次请求。总请求数为20*100=2000 。如果勾选了永远，那么所有线程会一直发送请求，直到选择停止运行脚本。

## 4.逻辑控制器Logic Controllers

逻辑控制器允许您自定义 JMeter 用于决定何时发送请求的逻辑，其可以更改来自子元素的请求顺序。

- Test Plan

- - Thread Group

  - - Once Only Controller

    - - Login Request (an HTTP Request)

    - Load Search Page (HTTP Sampler)

    - Interleave Controller

    - - Search "A" (HTTP Sampler)
      - Search "B" (HTTP Sampler)
      - HTTP default request (Configuration Element)

    - HTTP default request (Configuration Element)

    - Cookie Manager (Configuration Element)

参考以上示例测试树，‎此测试的第一件事是登录请求，这个请求仅在第一次执行，后续迭代将会跳过。这是由于其受到‎‎ **Once Only Controller‎** 的影响。登录后，下一个采样器是加载搜索页面（想象用户登录的 Web 应用程序，然后转到搜索页面进行搜索）。这只是一个简单的请求，因此没有任何逻辑控制器过滤。‎

‎加载搜索页面后，我们要进行搜索，我们希望在每次搜索之间重新加载搜索页面本身，即这个过程实际上就是 4 个简单的 HTTP 请求元素：加载搜索、搜索"A"、加载搜索、搜索"B" 。因此，我们可以使用‎ **‎Interleave Controller** 迭代控制器，这个控制器每次测试都会传递一个子请求，然后保留其子元素的迭代（即它不会随机传递，能"记住"位置）。

注意到 **Interleave Controller > HTTP default request** ，这是迭代控制器里样本的 HTTP 请求默认值，我们可以想象下，"搜索 A"和"搜索 B"都是共享相同的路径信息的，比如 HTTP 请求域、端口、方法、协议、路径和参数以及其他可选项目，那我们就可以将这些信息提取，然后共同配置，而不用为每一个采样器单独配置，**HTTP default request** 就是这样一个工具。

‎最后一个元素是 ‎‎**Cookie Manager**。所有网络测试中都应添加 Cookie 管理器，否则 JMeter 将忽略 Cookie。通过在线程组级别添加它，我们确保所有 HTTP 请求将共享相同的 Cookie

更多：请参阅 ‎[‎内置逻辑控制器列表](https://jmeter.apache.org/usermanual/component_reference.html#logic_controllers)

## 5.采样器Samplers

‎采样器告诉 JMeter 将请求发送到服务器并等待响应，运行时，线程组会按照采样器出现在树上的顺序逐个进行处理，JMeter 取样器包括：‎

- ‎FTP 请求‎
- ‎HTTP 请求（也可用于 SOAP 或 REST 网络服务）‎
- ‎JDBC 请求‎
- ‎Java 对象请求‎
- ‎JMS 请求‎
- ‎JUnit Test 请求‎
- ‎LDAP 请求‎
- ‎邮件请求‎
- ‎操作系统进程请求‎
- ‎TCP 请求‎

## 6.测试片段Test Fragments

‎测试片段元件是与线程组元件处于同一级别的测试计划树上的特殊‎‎控制器‎‎类型。它与线程组不同，它不会被执行，除非它被‎‎ **Module Controller** 或 **Include Controller** 引用。‎此元素纯粹用于测试计划中的代码重用‎。

## 7.监听器Listeners

Jmeter中使用监听器元件收集取样器记录的数据并以可视化的方式来呈现。Jmeter有各种不同的监听器类型。

例如聚合报告：右键点击线程组，在弹出菜单（添加--->监听器--->聚合报告）中选择聚合报告。

- Label：请求的名称
- Samples：总共发给服务器的请求数量
- Average：单个请求的平均响应时间，单位是毫秒
- Median： 50%的请求的响应时间小于
- 90%Line： 90%的请求的响应时间小于
- 95%Line： 95%的请求的响应时间小于
- 99%Line： 99%的请求的响应时间小于
- Min： 最小的响应时间
- Max： 最大的响应时间
- Error%： 错误率=错误的请求的数量/请求的总数
- Throughput： 吞吐量即表示每秒完成的请求数
- Received KB/sec： 每秒从服务器端接收到的数据量
- Sent KB/Sec： 每秒从发送到服务器端的数据量

## 8.延时Timers

默认情况下，JMeter 线程会按顺序执行采样器，而不会暂停。我们建议您通过在"线程组"中添加可用的 **Timers** 来指定延迟。如果您不添加延迟，JMeter 可能会在很短的时间内提出过多请求，从而压倒服务器。‎

## 9.断言Assertions

‎断言允许您断言从正在测试的服务器收到的响应。使用断言，您基本上可以"测试"您的应用程序是否正在返回您期望的结果。‎例如，您可以断言查询的响应是否包含某些特定文本。‎您可以向任何采样器添加断言。例如，您可以在 HTTP 请求中添加一个断言，该请求检查文本 ‎‎"</HTML>" 。‎‎然后，JMeter 将检查 HTTP 响应中是否存在该文本。如果 JMeter 无法找到文本，则它将标记其为请求失败。‎

‎请注意，断言适用于其‎‎范围内‎‎的所有采样器。如要将断言限制为单个取样器，应将该断言添加为取样器的子项。‎‎要查看断言结果，请向线程组添加 **Assertion Listener**，另外失败的断言也将被统计并计入错误百分比中。‎

右键点击HTTP请求---->添加---->断言---->响应断言

- 要测试的响应字段： 响应文本、Document(text)、URL样本、响应信息、Response Headers等选项。虽然接口返回的是HTML页面，但对于Jmeter来说返回数据为文本，所以，这里可以勾选响应文本。
- 模式匹配规则： 包括、匹配、 Equals、Substring。这里只需要验证返回数据中是否包含主要的关键字，所以，这里勾选包括。
- 要测试的模式： 其实就是断言的数据。点击添加按钮，输入要断言的数据。

## 10.配置元件Configuration Elements

‎配置元件与采样器紧密配合。虽然它不发送请求‎‎，但它可以添加或修改请求。‎‎配置元素只能从放置元素的树枝内部访问。例如，如果您将 HTTP Cookie Manager 放置在 Simple Logic Controller 中，则 Cookie 管理器将仅对您放置在简单逻辑控制器内的 HTTP 请求控制器访问。Cookie 可访问"Web Page 1"和"Web Page 2"，但不能访问"Web Page 3"。‎

‎此外，树枝内的配置元素优先于"父"分支中的相同元素。例如，我们定义了两个 HTTP 请求默认元素，"Web Defaults 1"和"Web Defaults 1"。由于我们把"Web Defaults 1"放在 Loop Controller 中，因此只有"Web Page 2"才能访问它，其他 HTTP 请求将使用"Web 默认值 2"。

![](/images/drawingbed/img/202204291749981.png)

‎Configuration Elements 无论被放置在哪里，其都会在测试开始时处理，但我们还是建议该元素放在线程组的开头。‎

## 11.预处理器Pre-Processor Elements

预处理器在发出取样器请求之前执行某些操作。如果预处理器连接到采样器元件，则它将在该取样器元件运行之前执行。预处理器最常用于在示例请求运行前修改其设置，或更新未从响应文本中提取的变量。

## 12.后处理器Post-Processor Elements

处理在发出取样器请求后执行某些操作。如果后处理器连接到采样器元件，则它将在采样器元件运行后执行。后处理器最常用于处理响应数据，通常用于从中提取值。

## 13.变量

**Test Plan** 和‎‎ **User Defined Variables** 配置元件所定义的变量值可在启动时提供给整个测试计划。当你定义变量后，在测试计划中可以使用 **${HOST}**  或 **${THREADS}** 来使用变量

{{< details "参考文件" >}} 
1：[ JMeter 用户文档 ](https://jmeter.apache.org/usermanual/)
{{< /details >}} 