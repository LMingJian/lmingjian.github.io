---
title: 如何实现双击文本修改
date: 2022-05-25
author: LM
---

## 1.需求

不可编辑的 "文本框" 在双击后，变更为编辑状态。

## 2.实现

```javascript
$('.edit').on('dblclick', function(event){
    let e = event.target
    if(e.innerHTML.indexOf('type="text"') > 0 ){  //判断是否进入编辑状态
        return
    }
    let newdiv = document.createElement( 'input' )//创建一个input标签
    newdiv.type = 'text'                          //设置标签类型
    newdiv.className = 'edit-input'      
    newdiv.value = e.innerHTML                    //将原来文本内容赋值给input标签
    e.innerHTML = ''             //清除原来的内容
    e.appendChild(newdiv)        //将input标签添加到元素中
    newdiv.setSelectionRange(0, e.innerHTML.length)  //设置光标选中位置
    newdiv.focus()               //设置元素获得焦点，失去焦点触发onblur
    newdiv.onblur = blur         //设置失去焦点事件
    function blur(){
        e.innerHTML = this.value   //将文本内容重新赋值给元素
    }
    newdiv.onkeyup = function(key){ //设置回车时执行失去焦点事件
        if(key.key == 'Enter'){ newdiv.blur() }
    }
})
```

