---
title: 下载文件重命名
date: 2023-08-10
author: LM
---

## 1.需求

由于后端存储文件时使用的是唯一 ID ，此时，用户下载文件时若不进行重命名，则难以分别该文件是否正确。因此，需要将下载的文件如文档，文本，安装包等重名字。

## 2.实现

将下载的文件读取为文件流 Blob，在保存文件流时进行重命名。

```javascript
// 下载文件，并获得文件流
function getBlob(url) {
    return new Promise((resolve) => {
        var xhr = new XMLHttpRequest();
        xhr.open('GET', url, true);
        xhr.responseType = 'blob';
        xhr.onload = function() {
            if (xhr.status === 200) {
                resolve(xhr.response);
            }else{
                resolve(0);
            }
        };
        xhr.send();
    })
}

// 重命名并保存文件流
function saveAs(blob, file_name) {
    var link = document.createElement('a');
    link.href = window.URL.createObjectURL(blob)
    link.download = file_name;
    link.target="_blank";
    link.click();
}
```

