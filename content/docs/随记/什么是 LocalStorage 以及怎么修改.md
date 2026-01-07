---
title: 什么是 LocalStorage 以及怎么修改
date: 2024-12-25T10:33:30+08:00
author: LiangMingJian
---

# 什么是 LocalStorage

LocalStorage 是浏览器提供的一种持久化存储方案，主要是为了解决 Cookie 存储空间不足的问题，让用户得以在网页中长期保存数据。

与 Cookie 的存储空间只有 4KB 不同，LocalStorage 的存储空间一般可以去到 5 MB（不同的浏览器支持的大小各不同，但总是比 Cookie 大）。

# LocalStorage 的分类

LocalStorage 在浏览器中分成两类，分别是本地存储 LocalStorage 和会话存储 SessionStorage，这两种存储的区别注意在于其的作用域和生命周期。

**本地存储 LocalStorage**

- **接口**：在浏览器中可以通过 `localStorage` 或者 `window.localStorage` 进行读取。
- **作用域**：在相同协议，相同域名，相同端口的环境下。比如 `https://www.example.com` 和 `https://www.example.com/abcd` 能使用同一个 LocalStorage，但 `https://www.example.com:8080` 和 `https://www.example.com` 就不能使用同一个 LocalStorage（因为端口不同）。
- **生命周期**：理论上永久有效，只要用户不手动清空（清除浏览数据），则数据永远存在。

![](_images/drawingbed/img/Pasted%20image%2020260106100022.png)

![](_images/drawingbed/img/Pasted%20image%2020260106100316.png)

**会话存储 SessionStorage**

- **接口**：在浏览器中可以通过 `sessionStorage` 或者  `window.sessionStorage` 进行读取。
- **作用域**：SessionStorag 的作用域与 LocalStorage 的基本一致，都要求在相同协议，相同域名，相同端口的环境下，**但 SessionStorag 还要在同一窗口下**，即如果存在多个相同域名的浏览器标签页，每个标签页都会有自己的 SessionStorag，各个标签页的 SessionStorag 不互通。
- **生命周期**：关闭标签页即失效，只生存在当前浏览器标签页。

![](_images/drawingbed/img/Pasted%20image%2020260106100150.png)

![](_images/drawingbed/img/Pasted%20image%2020260106100226.png)

# LocalStorage 的数据结构

LocalStorage 使用标准的键值对（Key-Value）格式来存储数据，因此，开发者一般使用 JSON 字符串来传递数据。

需要注意的是，LocalStorage 存储的数据类型限定为字符串 String，即使用户传入时是 Int，也会在写入时被转换为字符串 Sting。

另外，LocalStorage 其实还可以存储图片的，只要把图片转换为 base64 然后写入就行，不过一般不建议存储太大的图片。

# LocalStorage 的增删查改

LocalStorage 支持通过 JavaScript 来像数据库一样进行增删查改。

**本地存储 LocalStorage**

```javascript
// 增加/修改数据
function setStorage(key, value) {
  localStorage.setItem(key, JSON.stringify(value));
}

// 删除数据
function removeStorage(key) {
  localStorage.removeItem(key);
}

// 查询数据
function getStorage(key) {
  const value = localStorage.getItem(key);
  return value ? JSON.parse(value) : null;
}

// 清空所有数据
function clearStorage() {
  localStorage.clear();
}
```

**会话存储 SessionStorage**

```javascript
// 增加/修改数据
function setSessionData(key, value) {
  sessionStorage.setItem(key, JSON.stringify(value));
}

// 删除数据
function removeSessionData(key) {
  sessionStorage.removeItem(key);
}

// 查询数据
function getSessionData(key) {
  const data = sessionStorage.getItem(key);
  return data ? JSON.parse(data) : null;
}

// 清空所有数据
function clearSessionData() {
  sessionStorage.clear();
}
```

# 拓展：LocalStorage 与 Cookie 的异同

**相同点**

- 数据都存储在用户浏览器中。
- 数据格式都是键值对形式。
- 都具有同源策略限制，不同协议，域名，端口无法访问彼此的数据。

**不同点**

|特点|LocalStorage|Cookie|
|---|---|---|
|‌存储空间‌|5MB|4KB|
|‌传输方式|不会跟随请求发送到服务器|每次请求都会自动发送到服务器|
|‌安全性‌|无过期时间，需要手动清理敏感数据|可以设置过期时间|
|操作性‌|提供封装好的增删查改方法|需手动封装增删查改方法|
|‌适用场景|需要存储的大量数据（用户偏好）|安全需求高的少量数据（身份认证）|
