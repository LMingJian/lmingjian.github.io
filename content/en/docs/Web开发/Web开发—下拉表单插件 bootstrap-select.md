---
title: 下拉表单组件 bootstrap-select
date: 2021-06-06
author: LM
---

## 1.引用

bootstrap-select 是使用 bootstrap CSS/HTML 框架制作的一个下拉表单组件，使用该组件可以很便捷的创建美观，易用的下拉表单。

```html
<link href="Content/bootstrap/css/bootstrap.min.css" rel="stylesheet" />
<link href="Content/bootstrap-select/css/bootstrap-select.min.css" rel="stylesheet" />

<script src="Content/jquery-1.9.1.min.js"></script>
<script src="Content/bootstrap/js/bootstrap.min.js"></script>
<script src="Content/bootstrap-select/js/bootstrap-select.min.js"></script>
```

## 2.使用

```html
<select class="selectpicker" multiple>
    <option value="1">广东省</option>
    <option value="2">广西省</option>
    <option value="3">福建省</option>
    <option value="4">湖南省</option>
    <option value="5">山东省</option>                            
</select>
```

## 3.取值

 `var value = $('#sel').val();` ，需要注意的是，如果是多选，这里得到的 value 变量是一个数组变量，形如 `['1','2','3']`

## 4.赋值

```javascript
$('.selectpicker').selectpicker('val', '1');
```

注意，所操作的值为 option 的 value 属性

在一些级联选择的使用场景中，若需要在赋值的时候触发组件的 change 事件，可以：

```javascript
$('.selectpicker').selectpicker('val', '1').trigger("change");
```

多选的赋值

```javascript
$('.selectpicker').selectpicker('val', ['1','2','3']).trigger("change");
```

## 5.复原

```javascript
$('#initializePartyAProject').on('click', function () {
      // 回到初始状态
      $('#party_a_project_name').selectpicker('val',['noneSelectedText']) 
      // 对 party_a_project_name 这个下拉框进行重置刷新
      $("#party_a_project_name").selectpicker('refresh');
});
```

{{< details "参考文件" >}} 
1：[ github.com-bootstrap-select @silviomoreto ](https://github.com/silviomoreto/bootstrap-select)  

2：[ blogs @懒得安分](https://www.cnblogs.com/landeanfen/p/7457283.html)
{{< /details >}}