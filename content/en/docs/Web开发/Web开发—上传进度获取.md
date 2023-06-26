---
title: 网页开发：上传进度的获取
date: 2022-01-25
author: LM
---

## 1.需求

文件上传时能实时显示文件上传进度。

## 2.实现：通过 javascript 的 XMLHttpRequest:progress 事件

```javascript
var formData = new FormData(); 
formData.append("file", document.getElementById('file').files[0]); 
formData.append("token", token_value);

var xhr = new XMLHttpRequest();
xhr.open('POST', '/uploadurl');
// 上传完成后的回调函数
xhr.onload = function () {
  if (xhr.status === 200) {
　　console.log('上传成功');
  } else {
  　console.log('上传出错');
  }
};
// 获取上传进度
xhr.upload.onprogress = function (event) {
  if (event.lengthComputable) {
    var percent = Math.floor(event.loaded / event.total * 100) ;
    // 设置进度显示
    $("#J_upload_progress").progress('set progress', percent);
  }
};
xhr.send(formData);
```

## 2.实现：通过 jQuery 封装的 xhr

```javascript
var formData = new FormData(); 
formData.append("file", document.getElementById('file').files[0]); 
formData.append("token", token_value); 

$.ajax({ 
    url: "/uploadurl", 
    type: "POST", 
    data: formData, 
    processData: false, // 不要对data 参数进行序列化处理，默认为 true
    contentType: false, // 不要设置 Content-Type 请求头，因为文件数据是以 multipart/form-data 来编码
    xhr: function(){
        myXhr = $.ajaxSettings.xhr();
        if(myXhr.upload){
          myXhr.upload.addEventListener('progress',function(e) {
            if (e.lengthComputable) {
              var percent = Math.floor(e.loaded/e.total*100);
              if(percent <= 100) {
                $("#J_progress_bar").progress('set progress', percent);
                $("#J_progress_label").html('已上传：'+percent+'%');
              }
              if(percent >= 100) {
                $("#J_progress_label").html('文件上传完毕，请等待...');
                $("#J_progress_label").addClass('success');
              }
            }
          }, false);
        }
        return myXhr;
    },
    success: function(res){ 
        // 请求成功
    },
    error: function(res) {
        // 请求失败
        console.log(res);
    }
}); 
```

