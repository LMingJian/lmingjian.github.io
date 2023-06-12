---
title: Django 的 CSRF 中间件
date: 2021-07-25
author: LM
---

## 1.CSRF 中间件

CSRF 是跨站请求伪造漏洞。在 Django 中，CSRF 保护中间件`django.middleware.csrf.CsrfViewMiddleware`默认在`setting.py`设置中激活。

在使用 POST 表单的任何模板中，如果表单用于内部 URL，请在元素内使用`csrf_token`标签，例如`<form method="post">{% csrf_token %}`，对于针对外部 URL 的 POST 表单，不应这样做，因为这将导致 CSRF 令牌被泄露，从而导致漏洞。

## 2.异步 POST 带 CSRF 令牌传递

对于任何 AJAX POST 请求，都需要将 CSRF 令牌作为 POST 数据传递进来，您可以获取这样的令牌。

```javascript
function getCookie(name) {
    let cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        const cookies = document.cookie.split(';');
        for (let i = 0; i < cookies.length; i++) {
            const cookie = cookies[i].trim();
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}
const csrftoken = getCookie('csrftoken');
```

最后需要在 POST 请求中设置请求头。

```javascript
const request = new Request(
    /* URL */,
    {headers: {'X-CSRFToken': csrftoken}}
);
fetch(request, {
    method: 'POST',
    mode: 'same-origin'  // Do not send CSRF token to another domain.
}).then(function(response) {
    // ...
});
```

