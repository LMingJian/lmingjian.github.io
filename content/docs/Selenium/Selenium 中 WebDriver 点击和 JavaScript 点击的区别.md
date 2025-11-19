---
title: Selenium 中 WebDriver 点击和 JavaScript 点击的区别
date: 2024-12-25T16:13:50+08:00
author: LiangMingJian
---

# 问题

在自动测试时，有时候会出现无法通过 Selenium WebDriver 点击元素，但是可以通过执行 JavaScript 来单击的问题。

```python
element = driver.find_element(By.ID, "myid")
# 执行失败
element.click()
# 正常执行
driver.execute_script("arguments[0].click();", element)
```

# 问题分析

这两种方法的本质区别在于对浏览器的操作上：

**WebDriver 点击的实现逻辑**

当 WebDriver 执行单击时，它会尽可能地模拟真实用户使用浏览器时发生的情况。

比如，网页有一个按钮元素 A，同时有一个透明 div 元素 B，B 完全覆盖 A，然后，我们要求 WebDriver 去点击 A。

此时，我们会发现执行结果是元素 B 首先接收点击事件。

这是因为元素 B 覆盖元素 A，如果用户尝试点击元素 A，那么元素 B 将会首先获取事件，A 会不会获取到点击事件取决于 B 处理事件的方式。

我们可以看出，WebDriver 的行为与实际点击元素 A 的行为是一样的。

**JavaScript 点击的实现逻辑**

与 WebDriver 不同，假如直接使用 JavaScript 来执行点击，那么这个点击事件不会重现用户点击 A 时候的情况。

JavaScript 会将事件直接发送到元素 A，元素 B 不会收到任何事件。

**这也就解释了为什么 JavaScript 单击在 WebDriver 单击不起作用时有效？**

正如上面所介绍的，WebDriver 会尽力模拟真实用户的操作。因此，当 HTML 包含真实用户无法交互的元素时，WebDriver 也不会允许你进行交互。

除了上面提到的元素重叠情况外，元素不可见，元素尺寸为 0，元素被隐藏，元素在下拉表单中的这些情况也有可能造成 WebDriver 点击失效。

此时，可以尝试使用 JavaScript 直接进行操作，因为它会将事件直接传递到元素，而不考虑元素的可见性。

# 什么时候使用 JavaScript 点击？

**在爬虫的时候使用，在测试的时候不使用。**

当你在使用 Selenium 进行自动化测试时，这里不建议你使用 JavaScript 点击。因为，测试要求重现用户对浏览器的所有操作，如果使用 JavaScript 直接点击，那你将无法发现哪些 GUI 异常的问题。

当然，如果你是在爬虫，那直接使用 JavaScript 就无所谓了，因为你不用考虑是否要复现用户操作，只需要快速的获取数据，因此使用 JavaScript 绕过 GUI 就不是问题。

# 拓展阅读

WebDriver 执行时，会尽可能模拟用户的行为，因此，当出现如下情况时 WebDriver 会出现报错。

- 当点击顶部元素不是目标元素或其后代元素时
- 当元素没有大小或是完全透明时
- 当元素被禁止输入或禁止点击时

WebDriver 与 JavaScript 的异同。

- JavaScript 始终执行事件操作，即使元素已禁用。
- WebDriver 会像真实用户一样发出所有事件（鼠标移动、悬停、点击）。
- JavaScript 只会发出代码要求的事件，如果目标元素还依赖于其他的事件，那如果这些事件没有一起发出，目标元素的行为可能会有所不同。
- JavaScript 发出的事件没有点击的坐标。

————————————

> [ webdriver-click-vs-javascript-click @stackoverflow ](https://stackoverflow.com/questions/34562061/webdriver-click-vs-javascript-click)
