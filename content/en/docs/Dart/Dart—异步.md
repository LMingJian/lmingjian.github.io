---
title: Dart 的异步编程
date: 2021-04-30
author: LMingJian
---

## 1.Dart是一种单线程语言

Dart 是一种单线程编程语言，如果任何代码阻塞线程执行都会导致程序卡死。为了避免此类情况出现，Dart 使用 Future 对象表示异步操作。

```dart
// Synchronous code
printDailyNewsDigest() {
  String news = gatherNewsReports(); // Can take a while.
  print(news);
}

main() {
  printDailyNewsDigest();
  printWinningLotteryNumbers();
  printWeatherForecast();
  printBaseballScore();
}
```

在上述示例代码中，存在一个问题函数 printDailyNewsDigest，该函数是阻塞的，在这之后的代码都必须等待 printDailyNewsDigest 结束才能继续执行。因此为了程序能及时响应，Dart 的作者使用异步编程模型 Future 处理可能耗时的函数。

## 2.什么是 Future

Future 表示在将来某时获取一个值的方式。当一个返回 Future 的函数被调用的时候，程序做了两件事情：

- 函数把自己放入队列和返回一个未完成的 Future 对象
- 当值可用时，Future 带着值变成完成状态。

## 3.async 和 await

async 和 await 关键字是 Dart 异步支持的一部分。他们允许你像写同步代码一样写异步代码和不需要使用 Future 接口。

```dart
import 'dart:async';

printDailyNewsDigest() async {
  String news = await gatherNewsReports();
  print(news);
}

main() {
  printDailyNewsDigest();
  printWinningLotteryNumbers();
  printWeatherForecast();
  printBaseballScore();
}
```

如上述示例代码，在这时 printDailyNewsDigest 虽然是第一个调用的，但是最后打印的。这是因为代码读取和打印内容是异步执行的。

在这个例子中，printDailyNewsDigest 调用 gatherNewsReports 并不会阻塞程序。gatherNewsReports 会把自己放入队列，返回一个 Future 让程序正常执行下去，在 gatherNewsReports 完成收集新闻过后程序再来进行打印。

下面的图展示代码的执行流程。每一个数字对应着相应的步骤

![](/images/drawingbed/img/202204291909670.png)

1. 开始程序执行
2. main 函数调用 printDailyNewsDigest，因为它被标记为 async，所有在该函数任何代码被执行之前立即返回一个 Future
3. 剩下的打印执行。因为它们是同步的。所有只有当一个打印函数执行完成过后才能执行下一个打印函数。例如：中奖号码在天气预报执行打印。
4. 函数 printDailyNewsDigest 函数体开始执行
5. 在到达 await 之后，调用 gatherNewsReports，程序暂停，等待 gatherNewsReports 返回的 Future 完成。
6. 当 Future 完成，printDailyNewsDigest 继续执行，打印新闻。
7. 当 printDailyNewsDigest 执行完成过后，最开始的 Future 返回完成，程序退出。
8. PS：如果 async 函数没有明确指定返回值，返回的 null 值的 Future

从上面的示例可以看出，async 和 await 在使用时具有以下的规则。

- await 必须在 async 方法中使用。
- async 方法中在 await 前面的代码会立即同步执行，直到碰到 await。
- await 的作用是等待所标记的方法获取返回结果。当代码跑到 await 时，程序其他部分立即停止，直到标记方法执行完成
- 当代码执行到 await，程序会立即返回一个 future。

## 4.错误处理

如果在 Future 返回时发生错误，你可能想捕获错误。async 函数可以用 try-catch 捕获错误。

```dart
printDailyNewsDigest() async {
  try {
    String news = await gatherNewsReports();
    print(news);
  } catch (e) {
    // Handle error...
  }
}
```

## 5.连续执行

你可以使用多个 await 表达式，保证一个 await 执行完成过后再执行下一个

```dart
main() async {
  await expensiveA();
  await expensiveB();
  doSomethingWith(await expensiveC());
}
```

{{< details "参考文件" >}} 
1：[ Dart异步编程-future  @方田 ](https://www.cnblogs.com/hygblog/p/9078608.html)
{{< /details >}}