---
title: 插件 toastr
date: 2024-12-25T16:15:03+08:00
author: LiangMingJian
---

# 概述

toastr 是一个简单易用的 HTML 消息提示组件，使用该组件可以便捷地创建各种提示词。

# 引用

```html
<script src="Content/jquery-1.9.1.min.js"></script>
<script src="<%=path%>/res/toastr/toastr.min.js"></script>

<link rel="stylesheet" href="<%=path%>/res/toastr/toastr.min.css">
```

# 使用

```javascript
// 常规消息提示，默认背景为浅蓝色  
toastr.info("你有新消息了!");  

// 成功消息提示，默认背景为浅绿色 
toastr.success("你有新消息了!");  

// 警告消息提示，默认背景为橘黄色 
toastr.warning("你有新消息了!");  

// 错误消息提示，默认背景为浅红色 
toastr.error("你有新消息了!");  

// 带标题的消息框
toastr.success("你有新消息了!","消息提示");  

// 另一种调用方法
toastr["info"]("你有新消息了!","消息提示");
```

# 自定义

```javascript
toastr.options = {  
    closeButton: false,  // 是否显示关闭按钮
    debug: false,  // 是否为调试
    progressBar: true,  // 是否显示进度条
    positionClass: "toast-bottom-center",  // 消息框在页面显示的位置
    /*
    toast-top-left
    toast-top-right 
    toast-top-center 
    toast-top-full-width
    toast-botton-right  
    toast-bottom-left
    toast-bottom-center
    toast-bottom-full-width
    */
    onclick: null,  // 点击消息框自定义事件
    showDuration: "300",  // 显示动作时间
    hideDuration: "1000",  // 隐藏动作时间
    timeOut: "2000",  // 自动关闭超时时间
    extendedTimeOut: "1000",  
    showEasing: "swing",  
    hideEasing: "linear",  
    showMethod: "fadeIn",  
    hideMethod: "fadeOut"  
};  
```

{{< details "参考文件" >}} 
1： [ github-toastr @CodeSeven ](https://github.com/CodeSeven/toastr)
{{< /details >}}
