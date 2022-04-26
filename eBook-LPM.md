---
title: 电子书：《Linux Perf Master》
date: 2017-12-21 14:47:29
tags: [SRE,DevOps,Linux,Developer,eBook,Writing,Engineering]
categories: 出版物
---
## 摘要

![](http://riboseyim-qiniu.riboseyim.com/banner-LPM-201803.png)

- [《The Linux Perf Master》](https://riboseyim.com/2017/12/21/eBook-LPM/)
- [《The Cyber-Security Master》](https://www.gitbook.com/book/riboseyim/cyber-security-manual)
- [《The Machine Learning Master》](https://www.gitbook.com/book/riboseyim/machine-learning)

>If you cannot explain something in simple terms, you don't understand it. The best way to learn is to teach. —— Richard Feynman

<!--more-->

## 简介

《The Linux Perf Master》(暂用名) 是一本关于开源软件的电子书。本书与常见的专题类书籍不同，作者以应用性能诊断入手，尝试从多个不同的维度介绍以 Linux 操作系统为核心的开源架构技术体系。全书分为以下几个部分：
第一部分：性能诊断入门。介绍 Linux 性能诊断的入门方法，包括资源利用评估、性能监控、性能优化等工作涉及的工具和方法论，以 Stack Overflow 为例介绍一个真实的应用系统架构组成；
第二部分：基础设施管理工具。介绍 Ganglia,Ntop,Graphite,Ansible,Puppet,SaltStack 等基础设施管理 & 可视化工具；
第三部分：操作系统工作原理。介绍 Linux 操作系统工作原理（Not only Works,But Also How），从动态追踪技术的角度理解应用程序与系统行为；
第四部分：通信协议与网络工程。介绍基于 TCP/IP 协议的负载均衡技术，封包过滤技术和态势感知技术；微服务之后的挑战：分布式追踪系统（Planning);
第五部分：信息安全篇。介绍木马入侵、黑客攻击、防护与检测，IPv6 、容器等技术发展对安全工作的挑战；介绍信息安全法律；
第六部分：工程管理篇。尝试跳出 IT 视野讨论人才培养，DevOps 组织、效率和工程管理方法；
第七部分：社区文化篇。介绍黑客文化、开源作者、开发者社区和知识产权法，“技术首先是关于人的”（Technology is first about human beings）。

## 背景：我的第一本电子书

2016年7月份我已经提到，希望能实现一个小目标：出版一本专业书籍。目标虽小，实现不易。参阅了众多老司机的成功经验，我决定还是先整理一本电子书出来。《Linux Perf Master》Edition 0.1 在 **2017-02-10** 首次发布于 GitBook 平台，主题以 Linux 性能为核心，覆盖评估诊断、监控、优化的工具和方法论，还补充了几个参考案例。该书编辑过程中，早期没有使用 Markdown 发表的文章，没办法做到一键复用，必须再次进行繁琐的排版。另外，个人也不推荐使用编辑器：GitBook Editor for Mac ，它使用起来不太友好，也很容易崩溃。也不推荐在本地搭建一套自己的GitBook服务端，对个人用户来说过于繁琐。我的方式是“本地编写+自动同步”的方式：Git + Markdown真是珠联璧合、威力无穷。更多细节请查看：[我的写作工具链（持续更新）](https://riboseyim.github.io/2017/06/03/Writing-WriterToolChain/)。

#### GitBook 地址

- https://riboseyim.gitbook.io/perf
- https://www.gitbook.com/book/riboseyim/linux-perf-master/details

![](http://riboseyim-qiniu.riboseyim.com/ebook-linuxperfmaster-1.png)

## Amazon 上架
- 缺少资源：代理人

## 国内出版
- 缺少资源：出版社

- [《Linux Perf Master》](https://www.gitbook.com/book/riboseyim/linux-perf-master/details)：Edition 1.0 | 2016 | 电子工业出版社 | 选题阶段枪毙

## GitBook 访问数据
|时间点|订阅用户数|Downloads|Unique visitors|Page Views|说明|
|-----|-----|-----|-----|-----|-----|
|201701|-----|-----|-----|-----|GitBook Edition 0.1|
|20170630|135|4,206|4,936|-----|GitBook Edition 0.2|
|20170830|154|4,503|5,989|23,505|-----|
|20170930|157|4,553|6,471|24,944|-----|
|20171230|187|4,821|7,708|29,052|GitBook Edition 0.3|

## 下载

国内用户访问GitBook不太稳定，提供百度云快捷下载，同时提供了pdf、mobi、ePub三种格式。
- [Edition 0.4 20180714](https://pan.baidu.com/s/1C20TAKtYxXeRkTjNy43WOQ)
- [Edition 0.3 20171225](https://pan.baidu.com/s/1bppqKdL)
- [Edition 0.2 20170701](https://pan.baidu.com/s/1i4VsrbR)
- [Edition 0.1 20170210](https://pan.baidu.com/s/1bpGdzFT)

## 历史版本

**基本原则：持续发布，争取做到每四个月发布一个新版本**

#### Edition 0.4 20180714
- mod "网络工程篇"调整为分布式系统专题
- mod Pcap、sFlow 调整到Cyber-Security专题
- add 应用监控与可视化;LinkedIn Kafka Monitor
- add 应用监控与可视化;2018 Docker 用户报告
- add PostgreSQL 数据库
- add 案例：基于 Kafka 的事件溯源型微服务
- add 分布式追踪系统体系概要
- add 开源分布式跟踪系统 OpenCensus
- add 从作坊到工厂的寓言故事
- add 基础设施标准化：部署和配置管理
- add macOS vs Linux Kernels ？
- add IT 工程师养生指南

#### Edition 0.3  20171225
- 修订 Linux 快速性能诊断三篇、gRPC
- 监控数据可视化：Graphite、GIS
- How Linux Works:内存管理
- 调整部分章节顺序

#### Edition 0.2  20170701
- Linux 入门命令100条
- How Linux Works: Kernel Space & User Space Init
- 动态追踪技术：strace,gdb,ftrace,bcc,BPF
- 基于数据分析的网络态势感知
- Cyber-Security:IPv6,Web Headers,香港CSTCB

#### Edition 0.1  20170210
- 第一个 GitBook 版本，主要为 2016 年内容合辑
- 基于Linux单机负载评估
- 新一代Ntopng网络流量监控—可视化和架构分析
- 基于LVS的AAA负载均衡架构实践
- Linus Torvalds: The mind behind Linux


## [读者反馈信息](https://riboseyim.github.io/2017/07/05/Writing-Reader-Question/)
