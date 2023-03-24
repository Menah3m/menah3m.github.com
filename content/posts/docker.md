---
weight: 1
title: "docker 的一点小知识"
subtitle: ""
date: 2022-07-28T16:42:48+08:00
lastmod: 2023-03-16T10:19:48+08:00
draft: false
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: ["Docker","云原生"]
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



关于 Docker 的一些知识。

<!--more-->

## 容器

#### 容器与虚拟机的区别

![image.png](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/20230324151645.png)




## 如何在不重启正在运行的 docker 容器的情况下重启 docker 服务
1. 修改 /etc/docker/daemon.json
```shell
# 配置 live-restore 为 true 
 {
  "live-restore": true
 }
```
2. 重载 docker 配置
```shell
 kill -SIGHUP $(pidof dockerd)
```
3. 检查配置是否生效
```shell
 docker info |grep -i live
 # Live Restore Enabled: true
```
4. 重启 docker 服务
```shell
 systemctl restart docker
```
