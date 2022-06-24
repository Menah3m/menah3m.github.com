---
title: "Cerebro 的安装"
subtitle: ""
date: 2022-06-24T10:36:26+08:00
lastmod: 2022-06-24T10:36:26+08:00
draft: true
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: ["cerebro","Elasticsearch"]
categories: ["Guide"]

featuredImage: ""
featuredImagePreview: ""

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false
toc:
  enable: true
  auto: true
code:
  copy: true
  maxShownLines: 50
math:
  enable: false
share:
  enable: true
comment:
  enable: true
library:
  css:
    #someCSS = "some.css"
    #located in "assets/"
    #Or
    #someCSS = "https://cdn.example.com/some.css"
  js:
    #someJS = "some.js"
    #located in "assets/"
    #Or
    #someJS = "https://cdn.example.com/some.js"
seo:
  images: []
---

cerebro 是一款开源的用于管理 Elasticsearch 的 Web 端管理工具。 

<!--more-->



## 工具介绍



**项目地址**

`https://github.com/lmenezes/cerebro`



**项目依赖**

Java 11 及以上版本



## 安装



**安装方式**

- 软件包安装

  下载地址：`https://github.com/lmenezes/cerebro/releases`

- Docker（推荐）

  执行命令 `docker run --name cerebro -d -p 9000:9000 lmenezes/cerebro:0.8.4`



