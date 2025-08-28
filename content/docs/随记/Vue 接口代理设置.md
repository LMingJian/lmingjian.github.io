---
title: Vue 接口代理设置
date: 2025-06-27T16:14:06+08:00
author: LiangMingJian
---

# 问题

Vue 项目本地调试时，如果需要调用接口，但接口不在本地 lcoalhost，那么就需要使用 proxy 代理，将本地域名代理至目标接口地址。

# 实现

修改文件：`vue.config.js`

```javascript
devServer: {
  proxy: {
      '/': {
          target: 'http://e.dxy.net',  // 后台接口域名
          ws: true,        //如果要代理 websockets，配置这个参数
          secure: false,  // 如果是https接口，需要配置这个参数
          changeOrigin: true,  //是否跨域
      }
  }
}
```
