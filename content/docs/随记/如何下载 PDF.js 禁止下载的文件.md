---
title: 如何下载 PDF.js 禁止下载的文件
date: 2026-09-20T11:14:18+08:00
author: LiangMingJian
---

# 什么是 PDF.js

PDF.js 是一个通用的、基于 Web 标准的平台，用于解析和呈现 PDF 的 JavaScript 库。

当你在网站上看见如下图所示的 PDF 展示器时，你应该要知道这是一个使用了 PDF.js 的展示器。

![](_images/drawingbed/img/Pasted%20image%2020260920111840.png)

# 禁用的原理

某些网站在使用 PDF.js 为用户提供 PDF 预览功能时，为了限制用户下载文件，通常会使用以下的一种或多种方法组织下载。

1. 禁用右键菜单
2. 隐藏下载按钮
3. 移除 PDF.js eventBus 的 openfile, save 和 print 事件
4. 移除 PDF.js 相关下载和打印的方法，避免从 Console 调用

当你在 PDF 展示器中看不见下载和打印按钮时，你应当要知道网站开发者不想你下载文件。

![](_images/drawingbed/img/Pasted%20image%2020260920112226.png)

![](_images/drawingbed/img/Pasted%20image%2020260920112250.png)

# 绕过方法

从简单到复杂，请逐个方法进行尝试。

- 在 F12 控制台 Console 中执行方法 `this[0].PDFViewerApplication.download()`
- 在 F12 控制台 Console 中执行方法 `this[0].PDFViewerApplication.downloadManager.download()`

> `this[0]` 是常规的 `PDFViewerApplication` 对象存储位置，但可能会有网站会存放到其他 `this`，请尝试修改序号。

上述方法是通过直接执行 PDF 展示器对象中的下载方法来实现下载。

这种方法适用于那些只注释隐藏打印下载按钮，但没有注释下载方法的网站。

但当网站开发者发现这个问题，在网站中注释掉 PDF.js 中相关下载代码时，这个方法便不能使用了。

![](_images/drawingbed/img/Pasted%20image%2020260920113324.png)

此时，我们可以通过自己构建下载函数，然后恢复 `PDFViewerApplication` 对象的下载功能。

参考源码，我们可以这样构建一个下载函数：

```JavaScript
function download(blobUrl, filename) {  
	const a = document.createElement("a");  
	if (!a.click) {  
		throw new Error('DownloadManager: "a.click()" is not supported.');  
	}  
	a.href = blobUrl;  
	a.target = "_parent";  
	if ("download" in a) {  
	a.download = filename;  
	}  
	(document.body || document.documentElement).append(a);  
	a.click();  
	a.remove();  
}  
  
const data = await this[0].PDFViewerApplication.pdfDocument.getData();  
const blob = new Blob([data], {  
	type: "application/pdf"  
});  
const blobUrl = URL.createObjectURL(blob);  
const filename = this[0].PDFViewerApplication.contentDispositionFilename;  
  
console.log(blobUrl);  
console.log(filename)  
download(blobUrl, filename);
```

在 F12 控制台 Console 中，粘贴这个方法，回车执行，即可完成文件下载。

————————————

> 参考原文：[ 下载 PDF.js 禁止下载的文件 ](https://blog.zhanganzhi.com/zh-CN/2023/08/6946cd043888/)
