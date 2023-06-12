---
title: Django 文件上传的实现
date: 2022-01-23
author: LM
---

## 1.实现的过程

获取文件二进制数据 > 通过接口传递 > 写入服务器保存。

## 2.获取文件二进制数据

`<input type="file">`，使用 `type="file"` 的 `input` 元素使得用户可以选择一个或多个文件打开。附加属性：`accept, capture, files, multiple`。

### a.accept

accept 属性是一个字符串，它定义了文件 input 应该接受的文件类型。这个字符串是一个以逗号为分隔的**唯一文件类型说明符**列表。

```html
<input type="file" id="docpicker"
  accept=".doc,.docx,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document">
```

**唯一文件类型说明符**可使用如下格式：`.jpg .pdf`；一个不带扩展名的 MIME 类型字符串；字符串 `audio/*`， 表示任何音频文件；字符串 `video/*`，表示任何视频文件；字符串 `image/*`，表示任何图片文件。

### b.capture

capture 属性是一个字符串，当 input 打开的内容是图片或视频时，则它指定了使用哪个摄像头去捕获这些数据。

### c.files

FileList 对象为每个已选择的文件。如果 multiple 属性没有指定，则这个列表只有一个成员。

 可以通过`const numFiles = files.length;`来获取 FileList 列表的长度，可以像数组一样简单地访问文件列表来检索各个 File 对象。

```javascript
for (let i = 0, numFiles = files.length; i < numFiles; i++) {
  const file = files[i];
  // ...
}
```

### d.multiple

复数，是否允许选择多个。

## 3.通过接口传递

在获取 File 数据后，我们通过接口上传文件。

```javascript
// 选择 input 的 files，由于不允许复数，因此可以直接取 FileList 的第一个数据
const selectedFile = document.getElementById('input-file').files[0];
// 构建 FormData 数据，将文件数据写入 FormData
var fileData = new FormData();
fileData.append('avatar', selectedFile);
// 通过接口传递
$.ajax({
    type: 'post',
    url: '/api/upload',
    cache: false,
    // 数据不需要编码，contentType 置假
    contentType: false,
    processData: false,
    // 传递 FormData 数据
    data: fileData,
    headers: { 'X-CSRFToken': csrftoken },
    success: function(res) {
        if(res.status == '10000'){
            console.log('success')
        }
    }
})
```

## 4.写入服务器保存

通过 FormData 传递的文件数据，在服务器中可以通过 `request.FILES` 获取，然后再根据属性获取到指定的 File 数据。

```python
file_data = request.FILES
file = file_data.get('avatar')
```

编写函数，写入数据。

```python
def handle_uploaded_file(file):
    ext = file.name.split('.')[-1]
    if ext in ['html', 'htm', 'js', 'css', 'php']:
        return 0
    file_name = '{}.{}'.format(uuid.uuid4().hex[:10], ext)
 
    # file path relative to 'media' folder
    absolute_file_path = os.path.join('main/media', file_name)
 
    directory = os.path.dirname(absolute_file_path)
    if not os.path.exists(directory):
        os.makedirs(directory)

    with open(absolute_file_path, 'wb+') as destination:
        for chunk in file.chunks():
            destination.write(chunk)

    return file_name
```

## 5.文件的读取

通过指定`MEDIA_URL MEDIA_ROOT`的值，用户可以在前端读取上传的数据。

```python
# 在 setting.py 文件中写入如下配置
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
# 在 url.py 中配置路由
re_path(r'media/(?P<path>.*)$',serve,{'document_root':settings.MEDIA_ROOT}),
```

按上配置后，用户可以通过 `IP/media/文件名` 来读取上传内容。