---
title: JMeter 录制测试脚本
date: 2021-09-10
author: LM
---

## 1.JMeter设置代理服务器

JMeter 可以通过设置代理服务器来监听浏览器的所有请求并生成记录文件。

1. 在工具栏中选择 **Templates…** 
2. ![](/images/drawingbed/img/202204291755548.png)

3. 搜索并选择 **Recording** 脚本录制模板
4. ![](/images/drawingbed/img/202204291756926.png)

5. 此时出现如图完整的 **Test Plan**
6. ![](/images/drawingbed/img/202204291756394.png)

7. 在 **HTTP(S) Test Script Recorder** 中，点击启动即可，开启 Jmeter 服务器。
8. ![](/images/drawingbed/img/202204291756101.png)

## 2.证书

JMeter 启动代理服务器后会在 **JMETER_HOME/bin** 中生成一个证书 **ApacheJMeterTemporaryRootCA.crt** ，用户需要安装此证书，才可使用代理服务器。

打开任意浏览器，在设置中搜索证书，前往证书设置界面，导入证书至 **受信任的根证书颁布机构**。

## 3.浏览器配置

打开任意浏览器，在设置中搜索代理，前往代理设置界面，配置如下内容，启动代理。

- 地址：**localhost**
- Port：**8888**.

![](/images/drawingbed/img/202204291757581.png)

## 4.查看结果

尝试在浏览器中点击多个链接，关闭浏览器后，您可以在 **Thread Group -> Recording Controller** 中看到 JMeter 捕抓的HTTP请求内容

{{< details "参考文件" >}} 
1：[Apache JMeter - Apache JMeter HTTP(S) Test Script Recorder](https://jmeter.apache.org/usermanual/jmeter_proxy_step_by_step.html)
{{< /details >}}