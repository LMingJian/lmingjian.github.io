---
title: WebDriver click 与 JavaScript click 的区别
date: 2020-12-01
author: LM
---

## 问题

在自动测试时有时候会出现无法通过 Selenium WebDriver 点击命令来单击元素，但是可以通过执行 JavaScript 来单击的问题。

```python
element = driver.find_element_by_id("myid")
driver.execute_script("arguments[0].click();", element)
```

## 回答

这两种方法的本质区别在浏览器的操作上：

- WebDriver：当 WebDriver 执行单击时，它会尽可能地模拟当真实用户使用浏览器时发生的情况。比如您有一个元素 A，该按钮显示"单击我"，元素 B 是一个透明但具有其尺寸和设置的元素 B，B 完全覆盖 A。然后，您告诉 WebDriver 单击 A，WebDriver 将模拟单击，但结果却是 B 首先接收单击。这是因为 B 覆盖 A，如果用户尝试单击 A，则 B 将首先获取事件。A 最终是否会获得单击事件取决于 B 处理事件的方式。无论如何，在这种情况下，WebDriver 的行为与实际用户尝试单击 A 时的行为相同。
- JavaScript：假如您使用 JavaScript 来做 。单击此方法不会重现用户尝试单击 A 时真正发生的情况。JavaScript 将事件直接发送到 A，B 不会收到任何事件。

**这也就解释了为什么 JavaScript 单击在 WebDriver 单击不起作用时有效？**

正如我上面提到的，WebDriver 将尽力模拟当真实用户使用浏览器时发生的情况。事实是，DOM 可以包含用户无法与之交互的元素，并且 WebDriver 不允许您单击这些元素。除了我提到的重叠情况外，这还要求不能单击不可见的元素。我在其他一些问题中看到的一个常见情况是有人试图与 DOM 中已经存在的 GUI 元素进行交互，但仅在操作某些其他元素时才可见。这有时会与下拉菜单有关：您必须先单击显示下拉列表的按钮，然后才能选择菜单项。如果有人尝试在菜单可见之前单击菜单项，WebDriver 会犹豫，并说无法操作该元素。**如果此人随后尝试使用 JavaScript 进行操作，它将起作用，因为事件直接传递到元素，而不考虑可见性。**

## 何时应使用 JavaScript 单击？

如果你使用Selenium来测试一个应用程序，我对这个问题的回答是"几乎永远不会"。一方面，您的 Selenium 测试应重现用户对浏览器将执行哪些操作。以下拉菜单为例：测试应单击首先显示下拉菜单的按钮，然后单击菜单项。如果 GUI 出现问题，因为该按钮不可见，或者按钮无法显示菜单项或类似操作，则您的测试将失败，您将检测到 Bug。如果使用 JavaScript 单击四周，则无法通过自动测试检测这些 Bug。

我说"几乎从不"，因为使用JavaScript可能有例外。不过，它们应该非常罕见。如果使用 Selenium来刮取站点，则尝试重现用户行为就没那么重要了。因此，使用 JavaScript 绕过 GUI 不是问题。

## PS

WebDriver 执行的单击尝试在执行事件时尽可能接近模拟真实用户的行为，即使元素不可交互。当出现如下情况时WebDriver 会出现报错。

- 当单击坐标处顶部的元素不是目标元素或后代时
- 当元素没有正大小或它是完全透明的
- 当元素是禁用的输入或按钮时
- 当元素禁用鼠标指针时

WebDriver 与 JavaScript 的异同。

- JavaScript 将始终执行默认操作，如果元素已禁用，则充其量会以静默方式失败。
- WebDriver 会像真实用户一样发出所有事件（鼠标移动、鼠标悬停、鼠标悬停、单击...）。
- JavaScript 仅发出事件。页面可能依赖于这些额外的事件，如果它们未发出，它们的行为可能会有所不同。
- JavaScript 发出的事件没有单击的坐标。

{{< details "参考文件" >}} 
1：[ webdriver-click-vs-javascript-click @stackoverflow ](https://stackoverflow.com/questions/34562061/webdriver-click-vs-javascript-click)
{{< /details >}}