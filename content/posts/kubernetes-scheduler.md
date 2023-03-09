---
weight: 1
title: "Kubernetes 调度"
subtitle: ""
date: 2022-09-08T15:17:34+08:00
lastmod: 2022-09-08T15:17:34+08:00
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



Kubernetes Scheduler 的相关原理。

<!--more-->

## kube-scheduler

kube-scheduler 负责分配调度 pod 到集群内的节点上。它监听 kube-apiserver， 查询还未分配 Node 的 pod，然后根据调度策略为这些 pod 分配节点（即更新 pod 的nodeName 字段）

调度器需要考虑的诸多因素：

- 公平调度
- 资源高效利用
- QoS
- affinity 和 anti-affinity （亲和性）
- 数据本地化
- 内部负载干扰
- deadlines



### 调度器

kube-scheduler 调度分为两个阶段：

- predicate

  过滤不符合条件的节点

- priority

  优先级排序，选择优先级最高的节点



#### predicate 策略

1. PodFitsHostPorts 检查是否有host ports 冲突
2. PodFitsPorts
3. PodFitsResources 检查 Node 资源是否充足，包括孕育的 pod 数量、CPU、Memory 个数及其他
4. HostName 检查 pod.Spec.NodeName 是否和候选节点一致
5. MatchNodeSelector 检查候选节点的 pod.Spec.NodeSelector 是否匹配
6. NoVolumeZoneConflict 检查 volume zone 是否冲突



### priorities 策略

1. SelectorSpreadPriority 优先减少节点上属于同一个 Service 或 Replication Controller 的 pod 数量
2. InterPodAffinityPriority 优先将 pod 调度到相同的拓扑上，如同一个节点、rack、Zone 等
3. LeastRequestedPriority 优先调度到请求资源少的节点上
4. BalancedResourceAllocation 优先平衡各节点的资源使用
5. NodePreferAvoidPodsPriority alpha.kubernetes.io/preferAvoidPods 字段判断，权重为10000，避免其他优先级策略的影响



### 资源需求（resources）

#### requests 给调度器一个指标参考，最少需要多少

- cpu

  以 m 为单位，代表千分之一

- memory

#### limits 限制资源使用，代表最大能使用多少资源

- cpu

  以 m 为单位，代表千分之一

- memory
