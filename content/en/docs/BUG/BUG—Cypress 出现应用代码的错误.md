---
title: Cypress 出现 JavaScript 错误
date: 2021-09-18
author: LMingJian
---

## BUG 描述

在我们使用 Cypress 进行自动化时，有时网页自身的 JavaScript 会出现报错，此时 Cypress 也会捕抓这个异常，但这个异常并不是 Cypress 引起的，而我们也不需要这个异常。

## Resolution

我们可以在脚本开头添加以下代码，屏蔽这类异常。

```javascript
Cypress.on('uncaught:exception', (err, runnable) => {
    // returning false here prevents Cypress from
    // failing the test
    console.log('Jerry errors!');
    return false
})
```

