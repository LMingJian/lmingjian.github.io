---
title: 修复 Electron 在 Win7 出现白屏的问题
date: 2022-12-06
author: LMingJian
---

## BUG 描述

Electron 开发的软件在 Win7 平台下运行时会出现异常的白屏。

## Resolution

- 在启动时添加 `--disable-gpu` 参数，禁止硬件加速。
- 安装 `.NET Framework 4.6`。