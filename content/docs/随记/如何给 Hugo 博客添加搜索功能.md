---
title: 如何给 Hugo 博客添加搜索功能
date: 2024-12-25T10:13:59+08:00
author: LiangMingJian
---

# 利用 Hugo 自带的 index.xml 文件

由于 Hugo 生成的 index.xml 文件包含博客所有文章的标题，链接和概要，因此可以利用这个文件来进行内容搜索。JavaScript 实现如下。

```javascript
    if(window.location.pathname === '/404.html'){
	   query = window.location.search.substr(1)
	   if(query){
	       searchKey = query.replace('query=', '')
	       // $('#404').text(decodeURI(searchKey));
	       $('#404').text('搜索结果')
	       $.get('/index.xml', function(data){
	           items = data.getElementsByTagName("item");
	           var i = 0;
	           var node = ''
	           while ( i < items.length) {
	               txt = items[i].getElementsByTagName("title")[0].innerHTML + items[i].getElementsByTagName("description")[0].innerHTML;
	               if((txt.indexOf(searchKey)) > -1){
	                  var title = items[i].getElementsByTagName("title")[0].innerHTML;
	                  console.log(items[i].getElementsByTagName("description")[0].innerHTML);
	                  var link = items[i].getElementsByTagName("link")[0].innerHTML;
	                  node = node + '<p><a href="' + link + '">' + title + '</a></p>'
	               };
	               i++;
	           }
	           div = document.createElement("div")
	           div.innerHTML = node;
	           $('#404content').append(div)
	       });
	   }
    }
```

# 利用 Bing 搜索引擎实现

- Bing 搜索可以收录 GitHub Page ，因此可以在 Bing 中进行搜索。
- 需要注意的是 GitHub Page 禁止了百度爬取，因此 GitHub Page 的内容不能使用百度进行搜索。
- 使用`site:(https://your-page-name.github.io)` 可以将搜索范围固定在对应的 GitHub Page 中。

{{< details "参考文件" >}} 
1：[ 给 Hugo 站点添加搜索功能 - Go 语言中文网 ](https://studygolang.com/articles/27141?fr=sidebar)
{{< /details >}}
