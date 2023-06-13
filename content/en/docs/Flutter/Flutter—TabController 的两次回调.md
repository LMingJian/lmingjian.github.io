---
title: Flutter TabController 的两次回调
date: 2021-04-26
author: LM
---

## 1.TabController 的回调

Flutter 的 TabController 在点击切换 tab 时会回调两次，左右滑动切换 tab 则正常调用一次。

这是由于在点击切换 tab 时，程序会执行一个动画效果，而滑动切换时没有，因此在这个过程中多触发了一次 Listener，在设计时需要注意该问题，以避免监听出错。

