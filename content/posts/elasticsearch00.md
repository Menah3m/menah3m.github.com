---
weight: 1
title: "Elasticsearch 的安装和配置方法"
date: 2022-6-01T11:37:20+08:00
lastmod: 2022-06-22T06:34:33+08:00
draft: false
author: "Menah3m"
description: "简单记录下 Elasticsearch 的安装和配置"
images: []

tags: ["Elasticsearch"]
categories: ["Guide"]

lightgallery: true

---



简单记录下 Elasticsearch 的安装和配置。

<!--more-->

## 获取安装包

**官网地址**

https://www.elastic.co/cn/elasticsearch/

**下载方式**

- 软件包下载
- 包管理器下载

**使用版本**

V8.2.2



## Elasticsearch 的文件目录结构

- **bin**

  脚本文件，包括启动ES、安装插件、运行统计数据等

- **config**

  - elasticsearch.yml

    集群配置文件：user、role based等相关配置

- JDK

  Java运行环境

- **data**

  - path.data

    数据文件

- lib

  Java类库

- **logs**

  - path.log

    日志文件

- modules

  包含所有ES模块

- plugins

  包含所有已安装插件



## 运行 Elasticsearch

1. 下载并解压缩 Elasticsearch

2. 创建 es 用户并赋予权限（ES 默认不能使用 root 用户启动）

   ```shell
   useradd es
   passwd es
   chown -R es:es elasticsearch-8.2.2/
   chmod 770 elasticsearch-8.2.2/
   ```

3. 使用 es 用户运行 `bin/elasticsearch` 

4. 运行 `curl http://localhost:9200/` 

![result](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220623113554573-20220623130823304.png)





ES同时也支持使用 `Docker` 启动

```shell
docker run --name elasticsearch -d -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -e "discovery.type=single-node" -p 9200:9200 -p 9300:9300 elasticsearch:8.2.2
```



## JVM配置

### 修改 JVM

config/jvm.options

**建议**

- Xms 和 Xmx 设置成一样
- Xmx 不要超过机器内存的50%
- 不要超过 30GB



## 其他配置

### 如果需要外部能够访问，则需要修改 network.host

在 `config/elasticsearch.yml` 中修改 **network.host** 字段内容为 `0.0.0.0` ，并去掉注释标记



### 如果启动后访问时报错：ERR_EMPTY_RESPONSE

这是因为启动后增添了一些认证配置信息

需要将 **xpack.security.enabled**，**xpack.security.enrollment.enabled** 修改为 `false` 来关闭 ssl 认证



## 以多实例方式运行Elasticsearch



需要在 `config/elasticsearch.yml` 中设置 **cluster.initial_master_nodes** 字段值为 `myecs` （这里的 myecs 是第一个节点的名称）

执行如下命令即可：

```shell
bin/elasticsearch -E node.name=myecs -E cluster.name=mycluster -E path.data=myecs_data -d
bin/elasticsearch -E node.name=node1 -E cluster.name=mycluster -E path.data=node1_data -d
bin/elasticsearch -E node.name=node2 -E cluster.name=mycluster -E path.data=node2_data -d
```



关闭进程：

```shell
ps -ef|grep elasticsearch
kill -9 [pid]
```

