---
title: 如何实现 ctrl+v 粘贴图片
date: 2024-12-25T16:15:30+08:00
author: LiangMingJian
---

# 思路

要实现按下 ctrl+v 粘贴图片，可以监听 paste 事件，从 clipboardData 下面获取复制的数据，然后进行图片生成，或者上传。

# 解决

监听 paste 事件，然后使用 `getAsFile()` 的方法获取 blob 文件对象。

```javascript
document.addEventListener('paste', function (event) {
    if (event.clipboardData || event.originalEvent) {
        // 某些 chrome 版本使用的是 event.originalEvent
        var clipboardData = (event.clipboardData || event.originalEvent.clipboardData);
        if(clipboardData.items){
            var items = clipboardData.items,
            len = items.length,
            blob = null;
            for (var i = 0; i < len; i++) {
               console.log(items[i]);
               if (items[i].type.indexOf("image") !== -1) {
                  // getAsFile()
                  blob = items[i].getAsFile();
               }
           }
        }
    }
})
```

使用 blob 对象显示图片。

```javascript
var blobUrl=URL.createObjectURL(blob);
document.getElementById("imgNode").src=blobUrl;
```

使用 base64 显示图片。

```javascript
reader.onload = function (event) {
     // event.target.result 即为图片的Base64编码字符串
     var base64_str = event.target.result;
     document.getElementById("imgNode").src=base64_str;
}
reader.readAsDataURL(blob);
```

生成 formData 上传图片。

```javascript
var fd = new FormData();
fd.append("the_file", blob, 'image.png');
```
