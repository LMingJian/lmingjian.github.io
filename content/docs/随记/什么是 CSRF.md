---
title: 什么是 CSRF
date: 2024-12-25T10:33:13+08:00
author: LiangMingJian
---

# 什么是 CSRF 跨站请求伪造

CSRF (Cross Site Request Forgery)攻击，中文名：跨站请求伪造。其原理是攻击者构造网站后台某个功能接口的请求地址，诱导用户去点击或者用特殊方法让该请求地址自动加载。这样，当用户在登录状态下请求这个地址时，服务端就会以为这个非法操作是用户合法的操作。

# 如何避免

在客户端防范方面：对于数据库的修改请求，全部使用 POST 提交，禁止使用 GET 请求。在服务器端防范方面：一般的做法是在表单里面添加一段隐藏的唯一的 token (请求令牌)。比如：

- 服务端在收到路由请求时，生成一个随机数，在渲染请求页面时把随机数埋入页面（一般埋入 form 表单内）
- 服务端设置 setCookie，把该随机数作为 cookie 或者 session 种入用户浏览器
- 当用户发送 GET 或者 POST 请求时带上 csrf_token 参数（对于 Form 表单直接提交即可，因为会自动把当前表单内所有的 input 提交给后台，包括 csrf_token）
- 后台在接受到请求后解析请求的 cookie 获取 csrf_token 的值，然后和用户请求提交的 csrf_token 做个比较，如果相等表示请求是合法的
- 尽量少用 GET。假如攻击者在我们的网站上传了一张图片，用户在加载图片的时候实际上是向攻击者的服务器发送了请求，这个请求会带有 referer 表示当前图片所在的页面的  url。 而如果使用 GET 方式接口的话这个 URL 就形如：https://xxxx.com/gift?giftId=aabbcc&\_csrf_token=xxxxx，把 token 接在 URL 后面，那相当于攻击者获取了 csrf_token ，短时间内可以使用这个 token 来操作其他 GET 接口。

{{< details "参考文件" >}} 
1：[ CSRF 是什么？@饥人谷若愚 ](https://zhuanlan.zhihu.com/p/22521378)
{{< /details >}}
