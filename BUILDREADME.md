# Doks 客制化修改

> 原项目：[https://github.com/h-enk/doks](https://github.com/h-enk/doks)

## 什么是 Doks?

Doks 是 Hugo 的一个主题，能帮助用户构建安全、快速和 SEO 就绪的文档网站，支持轻松更新和自定义。

## 这项目是什么？

本项目是针对 Doks 的客制化修改，目的是让 Doks 更容易使用，界面更便于展示内容。

## 项目环境要求什么？

- [Node.js](https://nodejs.org/en) 版本大于 16.16.0，需要使用 npm 进行资源的管理和命令的执行。
- [Hugo](https://gohugo.io/) 版本大于 0.107.0，并且可执行文件路径在系统路径中。

## 如何使用 Doks?

### 1.资源安装

克隆项目到本地，进入项目文件夹，执行`npm install`。在执行命令后，可能需要执行`npm audit fix`进行修补。

### 2.启动调试

执行命令`npm run start`。

### 3.修改网站配置

在`.\config\_default\params.toml`中，修改以下内容可以变更网站标题等信息。

```toml
## Homepage
title = "Doks"
titleSeparator = "-"
titleAddition = "Modern Documentation Theme"
description = "Doks is a Hugo theme"
```

在同一文件中，修改以下内容可以变更网站组织信息。

```toml
## JSON-LD(A standardized format for user information)
# schemaType = "Person"
schemaType = "Organization"
schemaName = "Doks"
schemaAuthor = "Henk Verlinde"
```

在同一文件中，修改以下内容可以变更网站版权信息。

```toml
# Footer
footer = "Powered by <a class=\"text-muted\" href=\"https://www.netlify.com/\">Netlify</a>, <a class=\"text-muted\" href=\"https://gohugo.io/\">Hugo</a>, and <a class=\"text-muted\" href=\"https://getdoks.org/\">Doks</a></br>Article author"

# Feed
copyRight = "Copyright (c) 2020-2023 Henk Verlinde and Article author"
```

在`.\config\_default\config.toml`中，修改以下内容可以变更网站链接。

```toml
baseurl = "https://doks.netlify.app/"
canonifyURLs = false
disableAliases = true
disableHugoGeneratorInject = true
enableEmoji = true
enableGitInfo = false
enableRobotsTXT = true
paginate = 7
rssLimit = 10
```

### 4.修改主页内容

在`.\layouts\index.html`中，可以对网站主页根据需求进行修改。如下面所示内容，其中`{{ .Title }}{{ .Params.description }}`等内容会自动从 Markdown 文件`.\content\en\_index.md`的元数据中获取。

```html
{{ define "main" }}
<section class="section container-fluid mt-n3 pb-3">
  <div class="row justify-content-center">
    <div class="col-lg-12 text-center">
      <h1 class="mt-0">{{ .Title }}</h1>
    </div>
    <div class="col-lg-9 col-xl-8 text-center">
      <h1 class="mt-0">{{ .Params.description | safeHTML }}</h1>
      <h1 class="mt-0">{{ .Params.lead | safeHTML }}</h1>
      <!--<p class="lead">{{ .Params.lead | safeHTML }}</p>-->
    </div>
  </div>
</section>
{{ end }}
```

Markdown 文件元数据中对应内容通过`{{ ... }}`在网站上获取展示。

```markdown
---
title : "Hi there"
description: "Welcome to my blog"
lead: "＜（＾－＾）＞"
---
```

### 5.修改页眉菜单内容

在`.\config\_default\menus\menus.en.toml`中，可以对网站页眉的菜单链接进行修改。修改以下内容设置菜单名称和跳转链接。

```toml
[[main]]
  name = "Docs"
  url = "/docs/"
  weight = 10

[[main]]
  name = "Blog"
  url = "/blog/"
  weight = 20
```

### 6.文章内容的展示

在`.\content\en`文件夹中默认有着`docs, blog, link`三个文件夹，分别对应不同的界面，对应的界面模板文件位于`.\layouts`中。三界面分别展示如下的文档结构内容。

```tree
docs: /docs 展示一级目录的名称路径，/docs/help 展示该目录下包含文章名称路径，HTML 布局对应 .\layouts\docs 文件夹
  --help
    -_index.md
    -A.md
    -B.md
  --prologue
    -_index.md
    -A.md
  --_index.md
  
blog: /blog 展示一级目录的名称路径，/blog/welcome 展示文章内容，HTML 布局对应 .\layouts\blog 文件夹
  --say-hello
    -_index.md
  --welcome
    -_index.md
  --_index.md

link: /link 展示一级目录的名称路径，HTML 布局对应 .\layouts\link 文件夹
  --hugo
    -_index.md
  --_index.md
```

### 7.Github Action 部署

创建文件`.github\workflows\deploy.yml`，并填入以下代码。

```yml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.108.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass Embedded
        run: sudo snap install dart-sass-embedded
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v3
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build production website
        run: npm run build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2

```