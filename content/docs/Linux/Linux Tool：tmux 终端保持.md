---
title: Linux Tool：tmux 终端保持
date: 2025-09-25T19:18:05+08:00
author: LiangMingJian
---

# 前言

tmux（Terminal Multiplexer）是 Linux 中的终端复用工具，它提供强大的会话管理功能，允许用户在单个终端窗口中创建多个虚拟终端会话，并能在关闭终端窗口后，保持这些会话在后台运行。

#  安装

```shell
yum install tmux
```

# 命令使用

- **创建终端**：`tmux new` 或 `tmux new -s <name>`（指定终端名称）
- **列出终端**：`tmux ls` 或 `tmux list-sessions`
- **重连终端**：`tmux at` 或 `tmux at -t <name>`（连接指定终端、at 可以写成 attach）
- **关闭所有终端**：`tmux kill-server`
- **关闭指定终端**：`tmux kill-session -t <name>`

# 终端控制

tmux 虚拟终端的控制操作类似于 `vi` 或 `vim` 使用 ESC 切换命令模式，tmux 需要在输入控制命令前先按下前缀键 `Ctrl + B` 切换命令模式。

- **离开终端**：`Ctrl + B` 后 `D`
- **重命名终端**：`Ctrl + B` 后 `$` 或 `:rename-session <name>`
- **列出终端**：`Ctrl + B` 后 `S`
- **新建会话**：`Ctrl + B` 后 `:new` 或 `:new -s <name>`
- **打开前一个会话**：`Ctrl + B` 后 `(`
- **打开后一个会话**：`Ctrl + B` 后 `)`

# 窗口控制

tmux 虚拟终端的窗口是指同一终端的多个界面，允许打开多个窗口分别在一个终端内执行多个命令。同样，在输入控制命令前先按下前缀键 `Ctrl + B` 切换命令模式。

![](_images/drawingbed/img/Pasted%20image%2020251009115847.png)

- **创建新窗口**：`Ctrl+B` 后 `C`
- **切换前一个窗口**：`Ctrl+B` 后 `P`
- **切换后一个窗口**：`Ctrl+B` 后 `N`
- **切换最后一个窗口**：`Ctrl+B` 后 `l`
- **根据 ID 切换窗口**：`Ctrl+B` 后 `<ID: 0..9 >`
- **命名当前窗口** `Ctrl+B` 后 `,`
- **查找窗口**：`Ctrl+B` 后 `F`（有名称的窗口才能被找到）
- **关闭当前窗口**：`Ctrl+B` 后 `&`

# 布局控制

- **垂直分隔**：`Ctrl+B` 后 `%`
- **水平分割**：`Ctrl+B` 后 `"`
- **切换窗格**：`Ctrl+B` 后 `O` 或 `Q` 接窗格 ID
- **关闭窗格**：`Ctrl+B` 后 `X`
- **切换布局**：`Ctrl+B` 后空格
- **与上一个窗格交换位置**：`Ctrl+B` 后 `{`
- **与下一个窗格交换位置**：`Ctrl+B` 后 `}`
- **最大最小化当前窗格**：`Ctrl+B` 后 `Z`

# 其他命令

- **显示一个时钟**：`Ctrl+B` 后 `T`
- **显示所有快捷键**：`Ctrl+B` 后 `?`
- **进入命令行模式**：`Ctrl+B` 后 `:`

————————————

> [ tmux wiki ](https://github.com/tmux/tmux/wiki)
