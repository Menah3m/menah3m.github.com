---
weight: 1
title: "Gin 框架知识汇总"
subtitle: ""
date: 2022-08-01T09:52:17+08:00
lastmod: 2022-08-01T09:52:17+08:00
draft: true
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: ["Go","Gin"]
categories: ["Tips"]

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





<!--more-->



## ## 安装

#### 要求

- Go 1.13 及以上版本

#### 安装

```shell
go get -u github.com/gin-gonic/gin
```



#### 引入

```go
import "github.com/gin-gonic/gin"
```



## 基本使用



#### 初始化

```go
 r := gin.Default()
```



#### 路由处理

1. 基本操作

```go
r.GET(relative Path，handler)
r.POST(relative Path，handler)
r.PUT(relative Path，handler)
r.DELETE(relative Path，handler)
```

2. 路由分组

```go
apiv1 := r.Group(relative Path)
{
  apiv1.GET(relative Path，handler)
	apiv1.POST(relative Path，handler)
	apiv1.PUT(relative Path，handler)
	apiv1.DELETE(relative Path，handler)
}
```



