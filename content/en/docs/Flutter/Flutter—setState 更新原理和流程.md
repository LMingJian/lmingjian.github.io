---
title: Flutter setState 更新原理和流程
date: 2021-05-12
author: LM
---

## 1.Flutter 的状态类

Flutter 的状态类包括 `StatelessWidget`，无状态类，没有状态更新，界面一经创建无法更改。和 `StatefulWidget`，有状态类，当状态改变，调用 `setState()` 方法会触发 UI 状态更新。

## 2.是否可以刷新 mounted

调用 `setState()` 的界面必须没有调用过 `dispose()` 方法，否则会出现异常，可通过 `mounted` 属性来判断状态。

```
if (mounted) {
  setState(() {});
}
```

## 3.流程

### a.条件判断

- 生命周期判断
- 是否可以进行刷新：mounted

### b.添加脏链表 _dirty = true

- 脏链表是待更新的链表
- 更新过后就不脏了
- _active=false 的时候直接返回

### c.管理类

- 告诉管理类方法自己需要被重新构建
- owner.scheduleBuildFor(this)

### d.调用 window.scheduleFrame() =>native 方法

- RegisterNatives() 完成 native 方法的注册
- 最终会注册 vsync 回调。 等待下一次 vsync 信号的到来，
- 然后再经过层层调用最终会调用到 Window::BeginFrame()
- UI 的绘制逻辑是在 Render 树中实现的

### e.更新帧信号来临从而刷新需要重构的界面

- 在 drawFrame 中调用 buildOwner.buildScope(renderViewElement) 更新 elements

{{< details "参考文件" >}} 
1：[ Flutter的setState更新原理和流程 @flutter开发精选 ](https://zhuanlan.zhihu.com/p/271803637)
{{< /details >}}