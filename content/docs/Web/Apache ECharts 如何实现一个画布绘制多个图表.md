---
title: Apache ECharts 如何实现同一个画布绘制多个图表
date: 2025-12-03T10:19:26+08:00
author: LiangMingJian
---

# 需求

Apache ECharts 一个基于 JavaScript 的开源可视化图表库，如何实现下图所示的多个图表在同一画布展示的效果，同时要求图例能同时对两个图表中对应的数据进行控制。

![](_images/drawingbed/img/Pasted%20image%2020251203102127.png)

![](_images/drawingbed/img/Pasted%20image%2020251203102416.png)

# 实现逻辑

**图例组件（legend）**

legend 是 ECharts 的图例组件，用来标识图表数据所表示的含义，可以用于控制图表中系列数据的显示与否。

为了实现同一个图例能同时对两个图表进行控制的需求，因此需要在图例配置项 legend 中设置唯一的一个图例数据，而不能由代码自动生成。

```JavaScript
legend: {
    data: ['count1', 'count2', 'count3', 'count4', 'count5']
},
```

**网格组件（grid）**

grid 网格组件是实现一个画布绘制多个图表的**关键组件**，通过配置这个组件，可以人为的对画布进行区域划分。

grid 组件接收一个数组参数，在数组内，每一个画布都可以通过一个 json 进行配置，数组内有多少个 json，画布就划分为多少个网格。

比如下述示例代码，提供了两个 json，那么就代表画布被划分为两个网格。其中，第一个网格距离左边界 5%，距离右边界 50%；第二个网格距离左边界 55%，距离右边界 5%。

```JavaScript
grid: [
    { left: '5%', right: '50%' },
    { left: '55%', right: '5%' },
],
```

**设置坐标轴（xAxis，yAxis）**

xAxis，yAxis 是用于在画布中绘制坐标轴的参数。在多图表的实现过程中，如果目标加入的图表是具有坐标的，那边就需要将这个图表的坐标分别配置在 xAxis，yAxis 中。

xAxis，yAxis 支持一个数组参数，这个数组内通过多个 json 分别配置不同的网格内的坐标内容，开发者可以通过 **gridIndex** 来指定所配置的内容应用在哪一个网格内。

比如：设置两个坐标轴

```JavaScript
xAxis: [
    { type: 'category', gridIndex: 1, data: ['2024', '2025', '2026'] },
    { type: 'category', gridIndex: 0, data: ['2020', '2021', '2022'] }
],
yAxis: [
    { gridIndex: 1, type: 'value'},
    { gridIndex: 0, type: 'value' }
],
```

![](_images/drawingbed/img/Pasted%20image%2020251203110225.png)

比如：设置单个坐标轴

```JavaScript
xAxis: [
    { type: 'category', gridIndex: 0, data: ['2020', '2021', '2022'] }
],
yAxis: [
    { gridIndex: 0, type: 'value' }
],
```

![](_images/drawingbed/img/Pasted%20image%2020251203110318.png)

**设置直方图**

诸如直方图，折线图，散点图等带有坐标轴的图表都能使用下述的配置方法。

在多图表实例中，要配置这些图表的一大重点内容是让数据去到指定的坐标轴中。要实现这一点，那么开发者就需要在 series 的参数中指定数据 json 的 **xAxisIndex，yAxisIndex**。

**特别注意**，在多图表中，数据 series 需要使用 name 参数指定与 legend 图例中一致的名称，这样才能保证图例能对对应数据进行控制。

比如：两个坐标轴的数据填入

```JavaScript
series: [
    { 
      name: 'count1', type: 'bar', 
      xAxisIndex: 0, yAxisIndex: 0, 
      data: [10, 20, 0] 
    },
    { 
      name: 'count1', type: 'bar', 
      xAxisIndex: 1, yAxisIndex: 1, 
      data: [10, 20, 0]
    },
    { 
      name: 'count2', type: 'line', 
      xAxisIndex: 1, yAxisIndex: 1, 
      data: [25, 35, 0] 
    },
]
```

![](_images/drawingbed/img/Pasted%20image%2020251203111416.png)

特别的，**如果只有一个坐标轴**，那么即使不填入 xAxisIndex，yAxisIndex，代码也会自动的将对应直方图等具备坐标轴的数据填入这个坐标轴中。

比如：单个坐标轴的数据填入

```JavaScript
series: [
    { 
      name: 'count1', type: 'bar',
      data: [10, 20, 0] 
    },
]
```

![](_images/drawingbed/img/Pasted%20image%2020251203111620.png)

**设置饼图**

对于饼图，雷达图，仪表图等不需要的坐标轴的图表，其设置方法会比带坐标轴的方法更为简便，开发者只需在数据 series 中设置对应图表的**中心位置参数 center** 即可。

center 参数接收一个 `[水平位置, 垂直位置]` 数组参数，支持百分比或像素 `px`，比如：参数 `[25%, 50%]` 就表示图表应当在水平位置（从左到右）25% 处，垂直位置居中。

**特别注意**，为了图例控制，这里设置的数据也应当通过 name 指定与图例一致的名称。

比如：设置饼图在左边空白处

```JavaScript
option = {
  legend: {
    data: ['count1', 'count2', 'count3', 'count4', 'count5']
  },
  xAxis: [
    { type: 'category', gridIndex: 1, data: ['2024', '2025', '2026'] }
  ],
  yAxis: [{ gridIndex: 1, type: 'value' }],
  grid: [
    { left: '5%', right: '50%' },
    { left: '55%', right: '5%' }
  ],
  series: [
    {
      type: 'pie',
      radius: '30%',
      center: ['25%', '50%'],
      label: {
        show: true,
        formatter: '{d}%'
      },
      data: [
        { value: 356, name: 'count1' },
        { value: 104, name: 'count2' },
        { value: 520, name: 'count3' },
        { value: 74, name: 'count4' },
        { value: 595, name: 'count5' }
      ]
    }
  ]
};
```

![](_images/drawingbed/img/Pasted%20image%2020251203112720.png)

# 实现代码

文章开头所示需求的实现代码。

验证地址：[ Apache ECharts ](https://echarts.apache.org/examples/zh/editor.html)

```JavaScript
option = {
  legend: {
    data: ['count1', 'count2', 'count3', 'count4', 'count5']
  },
  xAxis: [
    {
      type: 'category',
      gridIndex: 0,
      data: ['2025-09', '2025-10', '2025-11', '2025-12', '2026-01']
    }
  ],
  yAxis: [{ gridIndex: 0, type: 'value' }],
  grid: [
    { left: '5%', right: '50%' },
    { left: '55%', right: '5%' }
  ],
  series: [
    {
      name: 'count1',
      type: 'bar',
      stack: 'total',
      data: [0, 129, 222, 10, 0]
    },
    {
      name: 'count2',
      type: 'bar',
      stack: 'total',
      data: [0, 129, 222, 10, 0]
    },
    {
      name: 'count3',
      type: 'bar',
      stack: 'total',
      data: [0, 0, 0, 0, 0]
    },
    {
      name: 'count4',
      type: 'bar',
      stack: 'total',
      data: [0, 0, 0, 0, 0]
    },
    {
      name: 'count5',
      type: 'bar',
      stack: 'total',
      data: [0, 0, 0, 0, 0]
    },
    {
      type: 'pie',
      radius: ['20%', '30%'],
      center: ['75%', '50%'],
      label: {
        show: true,
        formatter: '{d}%'
      },
      data: [
        { value: 356, name: 'count1' },
        { value: 104, name: 'count2' },
        { value: 520, name: 'count3' },
        { value: 74, name: 'count4' },
        { value: 595, name: 'count5' }
      ]
    }
  ]
};
```

![](_images/drawingbed/img/Pasted%20image%2020251203113331.png)
