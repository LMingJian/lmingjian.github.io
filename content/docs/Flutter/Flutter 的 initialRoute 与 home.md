---
title: Flutter 的 initialRoute 与 home
date: 2024-12-25T11:21:13+08:00
author: LiangMingJian
---

# 优先级

在 MaterialApp 中，initialRoute 和 home 都指向首页，但其优先级不同。

- 有 home，无 initialRoute，无 routes，只显示 home
- 有 home，有 initialRoute，无 routes，若 initialRoute 是 " / "，则正常显示 home；若不是 " / "，则程序会报错，但是仍然能显示 home
- 有 home，无 initialRoute，有 routes，若 routes 包含 " / "，则程序会报错；若 routes 不包含 " / " ，则显示 home
- 有 home，有 initialRoute，有 routes，若 routes 包含 " / "，则程序会报错；若 routes 不包含 " / " ，则会先显示 home，再显示 initialRoute ，从 initialRoute 可以返回到 home，这个特性可以用来实现启动页
- 无 home，有 initialRoute，有 routes，若 routes 包含 " / " ，则先显示 " / " ，再显示 initialRoute，从 initialRoute 可以返回到 " / " ，这个特性可以用来实现启动页
- 无 home，无 initialRoute，有 routes，若 routes 包含 " / " ，则显示 " / " ；若 routes 不包含 " / " ，则程序会报错
- 无 home，有 initialRoute，无 routes，程序报错

# 总结

- home 相当于 routes 中的 " / " ，因此当 routes 中有 " / " 时，不能设置 home
- 有 initialRoute 必须要有 routes
- home 和 initialRoute 同时存在时，会先显示 home，再显示 initialRoute，可以用来实现启动页
