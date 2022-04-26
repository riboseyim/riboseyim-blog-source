---
title: 数据可视化（六）常见的可视化仪表盘(DashBoard)
date: 2017-11-23 09:34:51
tags: [数据可视化,Developer,架构师]
categories: 工程技术
---
## 摘要
- 仪表盘（DashBoard）
- 决策路径 (Decision Path)
- ELK Stack（Elasticsearch、Logtash、Kibana）
- Graphite & Grafana
- Keen IO 、Librato、DataDog

<!--more-->

#### 仪表盘（DashBoard）
- 指标工具 (Metrics Tools)
- 可视化工具 (Visualization Tools)
- 日志管理工具 (Log Management tools)

#### 决策路径 (Decision Path)
1. OpenSource or SaaS ?
- 开源软件：需要自己安装和配置 ( Graphite and ELK stack )
- SaaS：需要改造现有程序、嵌入代码 ( Keen IO, Librato, and DataDog)

2. Analytics or Visualization
复杂分析: Keen IO 、 ELK stack
监控/指标呈现：Graphite（Grafana）、 DataDog

3. Budget and Environment
- 迁移成本：例如现有资产中已经有 Graphite 数据，采用Grafana可即时提升数据可视化效果
- 成本预算：例如日志分析器（log analyzer）的带宽和存储

## Options
- ELK Stack （Elasticsearch、Logtash、Kibana）
- Graphite & Grafana : [基于数据分析的网络态势感知](https://riboseyim.github.io/2017/07/14/Network-sFlow/)
- Keen IO
- Librato
- DataDog

|选项|价格|优点|弱点|
|-----------|-------|--------------|--------------|
|ELK Stack|免费（Elastic paid plans 提供不同级别的专家服务）|Strong Communities;Kibana 包含商业分析；ES 与 Kibana 易集成|安装配置；大规模使用时的机器成本|
|Graphite|免费|real-time graphs of numeric and time-differentiated data|数据采集和复杂分析能力弱；|
|Grafana|免费|支持多种数据源，提供丰富的插件|不提供数据存储，不提供数据采集|
|Keen IO|Free to $2000/月，按量收费|实时/归档数据可视化能力强；易于共享和提取数据|需要嵌入代码，依赖和拓展管理难度|
|Librato|按指标收费|监测和管理云应用，提供可以高度定制化的报表及告警功能|计费复杂，不提供数据采集，需要嵌入代码，依赖和拓展管理难度|
|DataDog|免费版，标准版（主机数量，$15台/月）|app、软硬件数据统一|目前不发展数据分析|

#### ELK Stack
Elasticsearch: 搜索和分析能力
Logstash: 日志聚合器(Aggregator)
Kibana：DashBoard

![](http://riboseyim-qiniu.riboseyim.com/DashBoards_ELK_Kibana.jpg)

#### Graphite & Grafana
- [Graphite](https://riboseyim.github.io/2017/12/04/Visualization-Graphite/): 开发语言（Python），支持数据存储、图形化和可视化，本身并不收集数据，需要和采集工具配合。
- Grafana: 开发语言（Go），提供了一个指标集的仪表盘，**可以将 Graphite 作为数据源（DataSource）**。

>Grafana provides many additional features and spiffy looking visuals to Graphite

![](http://riboseyim-qiniu.riboseyim.com/DashBoards_Grafana.jpg)

#### Keen IO
Keen IO is a **SaaS** analytics infrastructure platform.
![](http://riboseyim-qiniu.riboseyim.com/DashBoards_Keenio.jpg)

#### Librato
监测和管理云应用；
提供可以高度定制化的报表；
提供多样化的告警通知方式：邮件、HipChat、Campfire、HTTP Post

#### DataDog
DataDog is SaaS monitoring tool.
DataDog 主要围绕数据聚集和呈现，并不关注数据分析，即强调所有硬件、软件产生的数据实现汇聚统一。

![](http://riboseyim-qiniu.riboseyim.com/DashBoards_DataDog.jpg)

## 扩展阅读：数据可视化
- [数据可视化（一）思维利器 OmniGraffle 绘图指南 ](https://riboseyim.com/2017/09/15/Visualization-OmniGraffle/)
- [数据可视化（二）跑步应用Nike+ Running 和 Garmin Mobile 评测](https://riboseyim.com/2016/04/26/Visualization-BestAppMap)
- [数据可视化（三）基于 Graphviz 实现程序化绘图](https://riboseyim.com/2017/09/15/Visualization-Graphviz/)
- [数据可视化（四）开源地理信息技术简史（Geographic Information System](https://riboseyim.com/2017/05/12/Visualization-GIS/)
- [数据可视化（五）基于网络爬虫制作可视化图表](https://riboseyim.com/2017/05/12/Visualization-Charts/)
- [数据可视化（六）常见的数据可视化仪表盘(DashBoard)](https://riboseyim.com/2017/11/23/Visualization-DashBoard/)
- [数据可视化（七）Graphite 体系结构详解](https://riboseyim.com/2017/12/04/Visualization-Graphite/)
- [数据可视化（八）Program,Data and Classical Music](https://riboseyim.com/2018/12/16/Visualization-SocialNetwork/)
- [数据可视化（十）公共数据源列表](https://riboseyim.com/2018/01/15/Visualization-DataSource/)

## 参考文献
- [](http://blog.takipi.com/production-tools-guide/visualization-and-metrics/)
