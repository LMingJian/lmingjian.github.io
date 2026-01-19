---
title: ECharts 如何在界面缩放后自动重绘
date: 2026-01-09T09:38:40+08:00
author: LiangMingJian
---

# 需求

在 HTML 数字化大屏设计时，往往需要使用 Apache ECharts 来绘制图表。

但 ECharts 绘制的图表会有一个问题，即在 HTML 界面缩放后（Ctrl + 鼠标滚轮放大缩小），图表不会自动重绘，容易导致溢出。

![](_images/drawingbed/img/Pasted%20image%2020260109094630.png)

# 解决

通过监听 HTML 网页的缩放事件 `resize`，在缩放后调用 `echarts.js` 的 `resize()` 方法实现重绘。

```javascript
const chartDom = document.getElementById('chart-container');
const myChart = echarts.init(chartDom);
const option = {};
myChart.setOption(option);

// 监听窗口缩放事件，然后重绘
window.addEventListener('resize', () => {
  myChart.resize();
});
```
