---
weight: 1
title: "Kubernetes 中 Pod 的资源使用配置理解"
subtitle: ""
date: 2023-03-14T14:29:36+08:00
lastmod: 2023-03-14T14:29:36+08:00
draft: false
author: ""
authorLink: ""
description: ""
license: ""
images: []

tags: ["Kubernetes"]
categories: ["云原生"]

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

在使用 Kubernetes 部署应用时,CPU 及 Memory 的使用控制是很重要的一个点,合理的配置不仅可以帮助我们节省成本,也可以避免很多故障问题的发生。

<!--more-->

##  理解 requests 和 limits

Kubernetes 中最小的调度单位是 Pod，我们可以通过使用 requests 和 limits 字段来配置 CPU 及 Memory 的资源使用情况。

```yaml
resources:  
    requests:    
        cpu: "1"
        memory: "500Mi"
   limits:    
        cpu: "100m"
        memory: "1000Mi"
```

其中，CPU 的单位是 1 ,简单来说等同于一个 CPU 核心 ，1 = 1000m ,反之，500m 则代表使用半个 CPU 核心。
Memory 的单位是 Mi ，1Mi = 1024x1024。

在 Pod 生命周期开始时,调度器会根据每个节点的现有资源情况,通过和 requests 字段中设置的值做比较,来决定 Pod 在哪个节点创建,也就是说，requests 是在调度时发挥作用。而 limits 中设置的值对应 Pod 中容器的 Cgroups 设置,从而来限制该 Pod 的资源使用。

关于 limits,如果 CPU 资源使用超过了 limits 设置的值，Pod 并不会被直接杀死,而是会被限制使用。如果 Memory 资源使用超过了 limits 设置的值，Pod 会因 OOM 杀掉并重启。

## 理解 Pod 的 Qos 
Qos 全称是 服务质量等级，当 Pod 被创建时，K8s 会给它分配一个 Qos，共有三种可能的 Qos:
+ Guaranteed: Pod 中每个容器都有 CPU 和 Memory 的限制请求，且两者值相同（如果只明确了 limits 而未定义 requests，则 requests 的值等于 limits 值）
+ Burstable: Pod 中至少有一个容器有 CPU 或者 Memory 的限制请求，且不满足 Guaranteed 等级的要求
+ BestEffort: 没有配置任何的 requests 和 limits

如果因为资源不够，需要驱逐 Pod ，则驱逐的服务质量等级顺序如下：
> BestEffort --> Burstable --> Guaranteed

## 可设置 limits 和 requests 的作用域
可以限制 CPU 和 Memory 的资源使用的作用域有 Node、Namespace、Pod 三个

### Node
针对 Node，可通过配置 kubelet 实现
```shell
# 下面配置根据实际情况调整

--system-reserved=cpu=500m,memory=1.5Gi \
--eviction-hard=memory.available<1.5Gi,nodefs.available<20Gi,imagefs.available<15Gi \
--eviction-soft=memory.available<2Gi,nodefs.available<25Gi,imagefs.available<10Gi \
--eviction-soft-grace-period=memory.available=2m,nodefs.available=2m,imagefs.available=2m \
--eviction-max-pod-grace-period=30 \
--eviction-minimum-reclaim=memory.available=200Mi,nodefs.available=5Gi,imagefs.available=5Gi
```

### Namespace
通过修改 Namespace 实现
```yaml
apiVersion: "v1"
kind: "LimitRange"
metadata:
  name: "test-resource-limits"
  namespace: test
spec:
  limits:
    - type: "Pod"
      max:
        cpu: "2"
        memory: "2Gi"
      min:
        cpu: "30m"
        memory: "50Mi"
    - type: "Container"
      max:
        cpu: "2"
        memory: "2Gi"
      min:
        cpu: "30m"
        memory: "50Mi"
      default:
        cpu: "300m"
        memory: "600Mi"
      defaultRequest:
        cpu: "30m"
        memory: "50Mi"
      maxLimitRequestRatio:
        cpu: "30"

```

### Pod
通过修改 deployment 实现
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-deployment
spec:
  replicas: 1
  template:
    metadata:
      annotations:
        sidecar.istio.io/inject: "false"
      labels:
        app: demo
    spec:
      containers:
      - name: demo
        image: mritd/demo
        resources:
          limits:
            cpu: 100m
            memory: 100Mi
          requests:
            cpu: 10m
            memory: 10Mi
        ports:
        - containerPort: 80
```