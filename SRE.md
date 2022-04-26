---
title: SRE:Site Reliability Engineering
date: 2017-04-26 15:13:36
tags: [DevOps,SRE]
categories: 工程技术
---
## 摘要
- SRE Workflow
- Code define Config
- 拜占庭将军问题

<!--more-->

## Workflow
- [SREcon: Performance Checklists for SREs 2016 | Brendan Gregg's Blog ](http://www.brendangregg.com/blog/2016-05-04/srecon2016-perf-checklists-for-sres.html)
- [OS 造成的长时间非典型 JVM GC 停顿：深度分析和解决|庄振运](http://www.infoq.com/cn/presentations/a-long-period-of-atypical-jvm-gc-caused-by-os?utm_campaign=infoq_content&utm_source=infoq&utm_medium=feed&utm_term=global)

## Discuss
- [NASA:可以告知故障的机器](https://mp.weixin.qq.com/s?__biz=MjM5NjI5Mjc2MA==&amp;mid=2652990428&amp;idx=1&amp;sn=b1e5f1e7eacee672dda64bd62fec7e13&amp;scene=1&amp;srcid=041641asPQg5BIyJZkCfU6BC#rd)
COMSoL综合系统健康管理（Integrated System Health Management）软件的第一个版本于2003年在NASA艾姆斯研究中心（Ames Research Center）被开发出来，以此来监视一个试验型固液混合火箭发动机试车台。

- [腾讯数据中心:三大谷歌欧洲数据中心究竟如何做到100%自然冷却](https://mp.weixin.qq.com/s?__biz=MzA3MDQyNzEzOQ==&amp;mid=210032001&amp;idx=1&amp;sn=fe4f38c6bff29dadb56ca4a415bf0cba&amp;scene=1&amp;srcid=0913L7IQ4dMyawN0soqMMP2B#rd)

## Application

#### Code define Config

1. [SRE Team of Stack Overflow: DNSControl](http://blog.serverfault.com/2017/04/11/introducing-dnscontrol-dns-as-code-has-arrived)


2. [分类：网络协议](https://riboseyim.github.io/2017/05/26/Protocol/)

## Monitor
1. [beyond website monitoring the value of access logs](https://logmatic.io/blog/beyond-website-monitoring-the-value-of-access-logs/)


## Chris Jones:分布式共识系统

#### Minghua Ye：App Engine

[Minghua Ye](https://www.linkedin.com/in/minghua-ye-9346b062?trk=prof-samename-name)
scalable system

automated service discovery

**google protocol buffer**

消息协议，向后兼容
[](http://blog.csdn.net/caisini_vc/article/details/5599468)
[](http://www.ibm.com/developerworks/cn/linux/l-cn-gpb/index.html)

**core lib c++**

1. command-line flags
[](https://gobyexample.com/command-line-flags)

2. Logging

3. Googletest
	 diff log diff file
pages:[](https://github.com/google/googletest)
blogs:[](http://www.cnblogs.com/coderzh/archive/2009/04/06/1426755.html)

#### 分布式共识系统

CAP：无人值守的一致的高可用系统是不存在的
CA系统：分区难题   脑裂   如何判断主从
CP系统＋A：接受分区，在分区的情况下保持一致，牺牲一定损失

#### Zookeeper

- 拜占庭将军问题
稳定状态需要  3N＋1（拜占庭式失败）或2N＋1（非拜占庭式失败）个实例。即多进程达到一致   		

- 单点故障源
复制状态机（RSM），很久不动的冷备没有意义，风险更高。
应用：分布式cron系统
无状态微服务系统，先要有一个保障一致性（存储状态）的可靠服务。

## 扩展阅读：[DevOps 漫谈系列](https://riboseyim.github.io/2016/07/28/DevOps/)
- [Kanban 看板管理实践](https://riboseyim.github.io/2017/08/06/TeamWork-Kanban/)
- [DevOps 漫谈：基础设施部署和配置管理](https://riboseyim.github.io/2018/03/26/DevOps-Deployment/)
- [Linux 容器安全的十重境界](https://riboseyim.github.io/2017/11/12/DevOps-Container-Security/)
- [工程师的自我修养：全英文技术学习实践](https://riboseyim.github.io/2017/06/27/Technology-English/)

#### [DevOps 实践的本质是文化](https://riboseyim.github.io/2018/03/29/DevOps-Culture/)
- 学习力－团队生命之根
- 带领团队翻译书籍
- Don’t make me think
- 凡是被很多人不断重复的好习惯，要将其自动化整合到工具

## 参考文献
- [What is SRE (Site Reliability Engineering)?](https://www.oreilly.com/ideas/what-is-sre-site-reliability-engineering)
- [InfoQ:SRE是什么鬼 / 来自 Google DevOps 经验的落地实践](http://www.infoq.com/cn/presentations/experience-of-google-devops-landing-practice)
