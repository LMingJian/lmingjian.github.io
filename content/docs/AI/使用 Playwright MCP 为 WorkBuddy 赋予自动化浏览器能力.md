---
title: 使用 Playwright MCP 为 WorkBuddy 赋予自动化浏览器能力
date: 2026-07-06T09:56:39+08:00
author: LiangMingJian
---

# 需求

WorkBuddy 默认是不具备浏览器自动化能力的，但是我们可以通过为其配置 Playwright MCP Service，让 WorkBuddy 可以（通过浏览器驱动）操作浏览器。

Playwright MCP 服务器通过 [模型上下文协议 (Model Context Protocol)](https://playwright.dev/docs/getting-started-mcp) 提供浏览器自动化功能，使大语言模型（LLM）能够使用结构化的无障碍快照与网页进行交互 — 无需视觉模型。

> 当然，如果是视觉模型，那通过截图分析，结果可能更为准确。

# 环境

Playwright MCP 只支持 Node 环境，且 Node.js 要大于 20，请从 [https://nodejs.org/zh-cn](https://nodejs.org/zh-cn) 下载最新的 Node 安装。

# 安装

Playwright MCP 可以在任何的 MCP 客户端上运行，包括但不限于 VS Code、Cursor、Windsurf、Claude Code、Claude Desktop。

用户只需在 MCP 客户端的配置文件中添加以下项目，即可完成安装。

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

对于 WorkBuddy，我们先点击 “连应用”，然后点击 “更多连接器”，进入 “连接器” 管理界面。

![](_images/drawingbed/img/Pasted%20image%2020260706100717.png)

![](_images/drawingbed/img/Pasted%20image%2020260706100813.png)

这里的 “连接器” 就是我们所说的 MCP，点击右上角的 “自定义连接器” 按钮，点击 “配置 MCP”，将上面的 json 字段添加进去，保存后返回。

![](_images/drawingbed/img/Pasted%20image%2020260706101035.png)

此时，展开 Playwright MCP，点击刷新按钮，即可完成 Playwright MCP 的安装。

![](_images/drawingbed/img/Pasted%20image%2020260706101140.png)

> 对于其他的 MCP 客户端，你可以通过阅读官方文档来进行学习：[https://playwright.dev/mcp/clients/vscode](https://playwright.dev/mcp/clients/vscode)

# 使用

当 Playwright MCP 后，你便可以让 WorkBuddy 打开浏览器，操作浏览器，比如让 WorkBuddy 执行以下操作。

```
帮我用浏览器打开百度首页，点击新闻页，看看今天有什么热点信息
```

![](_images/drawingbed/img/Pasted%20image%2020260706101905.png)

![](_images/drawingbed/img/Pasted%20image%2020260706101839.png)

# 配置

## 无头模式

默认情况下，Playwright MCP 会让浏览器处于有头模式，让你看到发生了什么。如果你需要无头启用，隐藏浏览器显示，那么你可以在 json 配置中添加以下参数。

```
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--headless"]
    }
  }
}
```

## 指定浏览器

```
["@playwright/mcp@latest", "--browser=firefox"]
```

|参数|浏览器|
|---|---|
|`chrome`|Google Chrome（默认）|
|`firefox`|Mozilla Firefox|
|`webkit`|WebKit（Safari引擎）|
|`msedge`|Microsoft Edge|

## 设备仿真

```
["@playwright/mcp@latest", "--device=iPhone 15"]
```

## 窗口大小

```
["@playwright/mcp@latest", "--viewport-size=1280x720"]
```

> 更多的配置参数请查看：[https://playwright.dev/mcp/configuration/options](https://playwright.dev/mcp/configuration/options)
