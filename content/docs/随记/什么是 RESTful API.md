---
title: 什么是 RESTful API
date: 2024-12-25T10:33:45+08:00
author: LiangMingJian
---

# 概述

RESTful API 是一种基于 HTTP 协议的 API 设计风格，它只是一种**协议规范**。

RESTful API 通过统一的接口和资源操作规范，让不同的前端设备与后端服务器之间的交互更为简单、一致、以及可预测。

简单来说，RESTful API 就像一套通用的“语言规则”，让前端设备和后端服务器能高效进行沟通。

# RESTful API 设计原则

## 1.资源标识（API 的路由）

路由是指 API 的具体网址，即常说的 URL。

在 RESTful API 架构中，每一个 URL 代表一种资源，为此，URL 的命名应当使用名词复数，避免使用动词。

比如下述动物园的接口设计，应当使用 `/zoos, /animals, /employees` 来表示动物园列表，动物列表，员工列表，而不是使用 `/get_zoos, /get_animals, /get_employees` 等动词。

## 2.资源操作（HTTP 动词）

正如 1 所介绍，使用名词复数来表示资源，那么对资源的操作，增删查改，则使用 HTTP 动词来实现。

标准的 HTTP 动词有以下 7 个：

- GET：从服务器**获取资源**。
- POST：在服务器**新建资源**。
- PUT：在服务器**全量更新资源**。
- PATCH：在服务器**部分更新资源**。
- DELETE：从服务器**删除资源**。
- HEAD：从服务器**获取资源的元数据**。
- OPTIONS：从服务器**获取资源的可用信息**。

## 3.返回结果（HTTP 状态码)

服务器应当使用统一格式返回资源操作结果，并按 HTTP 协议标准返回对应的 HTTP 状态码。

在 RESTful API 架构中，一般建议使用 JSON 结构返回资源操作结果，一个简单的示例如下：

```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "id": 1,
    "name": "Zoos",
    "email": "zoos@example.com"
  },
  "requestId": "111111111111"
}
```

其中 `code` 表示状态码，`message` 表示提示，`dada` 是返回的数据，`requestId` 是请求的标识。

HTTP 状态码建议参照文章：[ 什么是 HTTP 响应状态码 ](https://zhuanlan.zhihu.com/p/1991529870262548430)

# 其他的一些规范

## 1.信息应当支持过滤

如果服务器返回的资源数量很多，则 API 应该提供参数，过滤返回结果。

比如使用以下列表参数，限定返回资源数量。

|名称     | 功能    |
| --- | --- |
|  ?limit=10   |  限定返回的资源数量   |
|  ?offset=10   |  从给定的位置开始返回数据   |
|  ?page=2&per_page=100   | 按分页大小给定返回的第几页数据    |
|  ?sortby=name&order=asc   |  按指定排序规则进行返回   |
|  ?type_id=1  | 按特定规则返回数据    |

## 2.API 应部署在专属域名下

应当尽量将 API 部署在专属域名之下，与静态资源服务器隔离。

```
https://api.example.com
```

当然，如果没有专属域名，也可以考虑在主域名下通过路由进行隔离。

```
https://example.org/api/
```

## 3.API 版本应放在 URL 中

应该将 API 的版本号放入 URL，进行标识。

```
https://api.example.com/v1/
```

————————————

参考文件：[ RESTful API 设计指南  @阮一峰 ](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
