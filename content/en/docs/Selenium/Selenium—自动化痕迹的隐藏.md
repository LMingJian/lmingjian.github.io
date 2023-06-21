---
title: Selenium 自动化痕迹的隐藏
date: 2023-03-20
author: LM
---

## 1.需求

在使用 Selenium + Webdriver 执行自动化任务时，Selenium 往往会在浏览器的 Navigator 以及 Document 对象里注入一些属性，如果被服务器检测到这些属性的存在，那我们就会被识别为机器人在访问，而无法正常获取内容。

一个简单的方法是，在配置好 Selenium 和 Webdriver 后调用浏览器打开 [https://bot.sannysoft.com/](https://bot.sannysoft.com/) 这个网站，这是一个可以展示浏览器属性，同时标出哪些属性可能会被检查为自动控制的浏览器的网站。通过这个网站我们可以判断我们的脚本操作是否被服务器检测到。

## 2.实现：伪装 navigator.webdriver 属性

在大多数情况下，服务器会对 `navigator.webdriver` 这个属性进行检测。在正常的浏览器中这个属性的值为 `undefined`或者 `false`，如果浏览器被 Selenium 之类的工具自动控制的话值将会变为 `true`，因此这是个检查自动操作的好方法。

根据 [MDN 上的介绍](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/webdriver)，`navigator.webdriver` 是个只读属性，所以不能通过覆盖值的方式来伪装。即使通过重写这个属性的 `getter` 来把它覆盖掉，服务器还是可以通过执行 `navigator.hasOwnProperty('webdriver')` 来进行检测。

```javascript
// 重写 navigator.webdriver 的 getter
Object.defineProperty(navigator, 'webdriver', {
    get: () => undefined,
});
```

![](/images/drawingbed/img/202303201636336.jpg)

所以，**解决这个问题最好的办法是在打开网页前把这个属性删除掉**。

```javascript
// 删除属性
delete navigator.__proto__.webdriver;
```

如果是 Chrome 系的浏览器，那么要删除这个属性，可以在浏览器的启动参数里添加 `--disable-blink-features=AutomationControlled`，然后在配置的 `excludeSwitches` 添加 `enable-automation`。

```python
content_copyfrom selenium import webdriver

options = webdriver.ChromeOptions()
options.add_argument('--disable-blink-features=AutomationControlled')
options.add_experimental_option('excludeSwitches', ['enable-automation'])
options.add_experimental_option('useAutomationExtension', False)
driver = webdriver.Chrome(options=options)
driver.get("https://example.com")
```

或者通过 Chrome DevTools Protocol 的命令在打开网页前执行删除的 JS 代码。

```python
driver.execute_cdp_cmd('Page.addScriptToEvaluateOnNewDocument', {
    'source': 'delete navigator.__proto__.webdriver',
})
```

而 Firefox 浏览器则没这么多办法。为了在打开网页前执行删除的 JS 代码， Firefox 浏览器可以使用扩展来实现这一需求。参考了 MDN 上的 [浏览器扩展入门](https://developer.mozilla.org/zh-CN/docs/Mozilla/Add-ons/WebExtensions)，构建元数据 `manifast.json` 文件和功能主体 `main.js` 文件

```json
// 元数据文件 
{

  "manifest_version": 2,
  "name": "WebdriverCleaner",
  "version": "1.0",

  "description": "Clean navigator.webdriver",

  "content_scripts": [
    {
      "run_at": "document_start",
      "matches": ["*://*/*"],
      "js": ["borderify.js"]
    }
  ]

}
```

```javascript
// js 文件
delete window.navigator.wrappedJSObject.__proto__.webdriver;
console.log('navigator.webdriver cleaned:', window.navigator.wrappedJSObject.webdriver);
```

将上述两个文件压缩成 ZIP 文件，放在脚本目录下，在启动浏览器时加载。

```python
driver.install_addon(r".\Borderify.zip", temporary=True) 
```

此时，再次访问 [https://bot.sannysoft.com/](https://bot.sannysoft.com/) ，可以发现成功伪装。

## 3.实现：携带正常使用的 Cookie 访问

有时候，即使成功伪装了浏览器的 `navigator.webdriver` 属性，但还是被服务器识别出来，无法正常访问。那么这时，可以尝试携带正常访问时的 Cookie 来跳过验证，往往会有奇效。

```python
cookie = {'name': f'{cookie_name}', 'value': f'{cookie_value}'}
browser.get(url)
browser.add_cookie(cookie)
browser.get(url)
time.sleep(2)
```

## 4.额外：Cloudflare 对机器人的检测

Cloudflare是一家网络性能和安全公司。他们为客户提供 WAF ( Web 应用程序防火墙 )，WAF 可以保护应用程序免受多种安全威胁，例如跨站点脚本 （XSS）、撞库和 DDoS 攻击，Cloudflare 可以保护网页不受机器人的访问。 Cloudflare 使用的机器人检测方法通常可以分为两类：被动和主动。被动机器人检测技术包括在后端执行的指纹检查，而主动检测技术依赖于在客户端执行的检查。

### Cloudflare 被动爬虫程序检测技术

- 僵尸网络检测：Cloudflare 维护着已知与恶意机器人网络相关的设备、IP 地址和行为模式的目录。任何怀疑属于这些网络之一的设备要么被自动阻止，要么需要等待以进一步检测。
- IP 地址信誉：用户的 IP 地址信誉（也称为风险评分或欺诈评分）会基于地理位置、ISP 和信誉历史记录等因素。例如，属于数据中心或已知 VPN 提供商的 IP 的声誉将比住宅 IP 地址差。站点还可以选择限制从其服务区域以外的区域访问站点，因为来自实际客户的流量不应来自那里。
- HTTP 请求标头：Cloudflare 使用 HTTP 请求标头来确定您是否是机器人。如果您有非浏览器的用户代理，那么您的抓取工具可以很容易地被识别为机器人。
- 指纹识别：Cloudflare 可以通过检测 TLS 握手期间，客户端 hello 消息中提供的字段，例如密码套件、扩展和椭圆曲线，以计算给定客户端的指纹哈希。将客户端请求中的用户代理标头与与存储的指纹哈希关联的用户代理进行比较。如果它们匹配，则安全系统假定请求源自标准浏览器。相反，则表明使用了自定义僵尸软件，请求会被阻止。

### Cloudflare 主动爬虫程序检测技术

- 验证码
- 画布指纹识别：画布指纹允许系统识别 Web 客户端的设备类别。设备类是指用于访问网页的系统的浏览器、操作系统和图形硬件的组合。由于机器人倾向于谎报其底层技术（通过其用户代理标头），因此 Cloudflare 可通过大量合法画布指纹 + 用户代理对的数据集来查找画布指纹与预期指纹之间的不匹配来检测设备属性欺骗（例如用户代理、操作系统或 GPU）。
- 事件跟踪：Cloudflare 将事件侦听器添加到网页。它们侦听用户操作，例如鼠标移动、鼠标单击或按键。大多数情况下，真实用户需要使用鼠标或键盘进行浏览。如果 Cloudflare 发现始终缺乏鼠标或键盘使用，他们可以假设用户是机器人。
- 环境接口查询：特定于浏览器的 API，时间戳 API，自动浏览器属性，沙盒检测。

{{< details "参考文件" >}} 
1：[ How to Bypass Cloudflare in 2023: The 8 Best Methods @ZenRows ](https://www.zenrows.com/blog/bypass-cloudflare)

2：[ 使用扩展在运行 Selenium 的 Firefox 浏览器中伪装 navigator.webdriver @akarin.dev ](https://akarin.dev/2022/02/15/disable-geckodriver-detection-with-addon/)
{{< /details >}}