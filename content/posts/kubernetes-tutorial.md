---
weight: 1
title: "Kubernetes 生产实践 Tips"
subtitle: ""
date: 2022-08-22T15:27:41+08:00
lastmod: 2022-08-22T15:27:41+08:00
draft: true
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: ["Kubernetes"]
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

## Kubernetes 中优雅停止 Pod

### 优雅停止

就像我们关机时一样，系统会完成一些清理操作之后再关闭，称之为优雅停止。

如果我们直接拔掉电源（硬中止），那么系统中的一些任务和操作将会来不及进行处理，有时会导致未知的问题。

### K8s 中停止 Pod 的逻辑

1. 当 Pod 被删除，会变为 Terminating 状态，Pod metadata 中 deletionTimestamp 字段会被标记删除时间

2. Kube-proxy 在周期内更新转发规则，将 Pod 从 service 的 endpoint 列表中删除，新的流量将不会转发到该 Pod
3. kubelet 开始销毁 pod
   - 如果 pod spec 中设置了 preStop hook，将会执行
   - 发送 `SIGTERM` 信号给容器内主进程，通知容器进行优雅中止
   - 等待 container 中的主进程完全停止，如果在 terminationGracePeriodSeconds 内（默认30s）还未停止，就发送 `SIGKILL`信号强制中止
   - 所有容器进程终止，清理 Pod 资源
   - 通知 ApiServer pod 销毁完成。

### 使用 preStop 字段

可以使用 preStop 字段来执行自定义的优雅停止逻辑

参考 [Kubernetes API 文档](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/#lifecycle-1)

## Kubernetes 健康检查

K8s 目前支持三种健康检查：

- **ReadinessProbe** 就绪检查 

  只有在就绪检查成功的前提下，才会接收流量，否则不会接收流量

- **LivenessProbe** 存活检查

  如果存活检查不成功，会自动重启容器

- **startupProbe** 启动检查

  让存活检查和就绪检查的开始探测时间延后，也就是说只有在启动检查成功的前提下，才会执行就绪检查或存活检查



#### 注意

1. 首选 HTTP 探测

2. 备选脚本探测

3. 尽量避免 TCP 探测

   因为只要端口能够响应 TCP SYN 包，TCP 检测结果都为 true ，当程序死锁时，不会影响端口，所以可能检测结果为 true，但实际上业务已经不健康了。

4. 所有提供服务的 container 都要加上 `ReadinessProbe`

5. 谨慎使用 LivenessProbe ，如果非用不可，则应该尽量放宽探测条件



## 合理设置 Request 和 Limit

#### 所有容器都应该设置 request

request 的值并不是指给容器实际分配的资源大小，而是用于给调度器用来判断每个节点上可分配资源还有多少。

通过节点总资源减去各容器定义的 request 之和，可以计算出节点上还有多少资源可供分配，从而做出合理的调度决策。

如果当前节点可供分配资源小于被调度的容器的 request ，则不会调度到该节点。如果无法合理调度，可能会出现节点资源利用不均的结果。



### 重要的线上应用应该如何设置

节点资源不足时，会触发自动驱逐，会根据优先级删除 Pod 以释放资源让节点自愈。

没有设置 request，limit 的 Pod 优先级最低，其次是 request 不等于 limit 的 Pod ，

request 等于 limit 的 Pod 优先级最高，不容易被驱逐。

所以建议将重要的线上应用的 request 和 limit 设置为一致的值。
