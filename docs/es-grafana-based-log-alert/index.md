# 基于 Grafana 和 Elasticsearch 对日志信息进行监控告警




<br>

在公司环境中，除了日志的存储和可视化需求外，我们常常需要对日志信息进行监控，关注某些报错信息，目前最常用的日志架构是 ELK ，即 Elasticsearch 、LogStash 和 Kibana 的组合，其中 LogStash 负责日志的采集和过滤，ElasticSearch 负责日志的存储，而 Kibana 则负责日志的可视化，而日志的告警则可以通过 Grafana 来实现。

<!--more-->



Grafana 是一个开源的数据可视化组件，经常被用作时间序列数据可视化和分析。它支持很多数据源，这其中包括 Prometheus 、 OpenTSDB 、Graphite 、InfluxDB 、Elasticsearch 等等。Grafana 支持从数据源中查询数据，支持组合多个不同数据源中的数据到一个展示仪表盘上，同时最新的 Grafana 也继承了告警组件，可以实现对数据源信息的监控和告警。



## 添加 Elasticsearch 数据源

#### step 1：登录 Grafana

登录你的 Grafana 管理界面

![image-20220607154314910](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607154314910.png)



#### step 2：添加数据源

点击左侧 `Configuration` （小齿轮），选择 `Data Sources`:

![image-20220607154452854](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607154452854.png)

点击右上角 `Add data source`:

![image-20220607154858470](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607154858470.png)

选择 `Logging & document database` 中的 `Elasticsearch`：

![image-20220607155041361](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607155041361.png)

填写对应信息，然后点击 `Save & Test`:

![image-20220607155802139](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607155802139.png)

返回如下信息则代表添加数据源成功：

![image-20220607155936503](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607155936503.png)



#### step 3：添加告警组件

点击左侧 `Alerting`（铃铛），选择 `Notification channels`:

![image-20220607161433083](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607161433083.png)

点击 `New channel`：

![image-20220607161537651](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607161537651.png)

填写以下信息，点击 `Save` 保存即可：

![image-20220607161834045](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607161834045.png)



#### step 4：添加仪表盘

点击右侧的 `Create`（“+”号），选择 `Dashboard`:

![image-20220607160311512](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607160311512.png)

点击 `Add new panel`:

![image-20220607160353511](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607160353511.png)

选择正确的数据源：

![image-20220607160524689](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607160524689.png)

填写查询条件：

![image-20220607162207607](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607162207607.png)

![image-20220607162329576](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607162329576.png)

点击 `Alert` 下的 `Create Alert`:

![image-20220607162437995](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607162437995.png)

设置告警策略：

![image-20220607162823288](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607162823288.png)

设置告警信息的接受者和告警提示信息：

![image-20220607163012338](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607163012338.png)

![image-20220607163109215](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220607163109215.png)
