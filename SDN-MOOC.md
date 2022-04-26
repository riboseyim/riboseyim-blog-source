---
title: SDN技术指南：推荐课程
date: 2019-06-07 13:04:43
tags: [SDN,网络协议,DevOps]
categories: 工程技术
---
## 摘要
- 《软件定义网络技术 Software Defined Networking,SDN》
- 主讲人：温州大学 | 施晓秋
- [中国大学MOOC链接](http://www.icourse163.org/learn/WZU-1205809832?tid=1206444220#/learn/content?type=detail&id=1211155089)
<!--more-->

## 第一讲 概述

- 主讲人：施晓秋

### 网络系统的生命周期

- 需求调研
- 规划设计
- 部署实施
- 运行维护

#### 需求调研
- 互连范围
- 互连规模
- 拟承载的应用：对象、类型、用户规模、服务质量

#### 规划设计

- 网络架构与拓扑
- IP 和 路由
- 安全和QoS策略

**体现一定的冗余**

#### 部署实施

#### 设备部署实施

- 上架
- 配置
- 连通

#### 系统部署实施

- 集成
- 上线
- 测试

#### 运行维护

- 网络状态的监控与管理：设备、用户、流量、应用；发现并排除网络故障与隐患


### 新一代网络架构

#### 传统网络主要问题

- 多元、多变的网络上层应用与业务

- 相对稳定的网络架构设计及系统运维

### SDN vs 计算机

| 计算机             | SDN        |
| ------------------ | ---------- |
| 裸机               | 基础设施层 |
| 操作系统           | 控制层     |
| 用户与应用程序接口 | API        |
|                   |            |

#### 架构变化

- 网络设备：解耦，简单化
- 网络管理：集中控制器，全局化
- 网络运维：SDN南向接口，自动化
- 网络应用：SDN北向接口，人性化

### Hisotry of SDN

#### 开创性人物

- Nick
- Martin
- Scott

#### 开创性论文

- [《Ethance:Taking Control of the Enterprise》](#)
- [《OpenFlow:Enablling Innovation in Campus Networks》](#)


#### 开创性项目

#### 起源：2005年

- 2005年，美国国家自然科学基金会：网络创新实验环境（Global Environment Networking Innovations,GENI），斯坦福大学 Clean-Slate项目：McKeown Group团队。

#### 初创期：2006-2011年 [学术界]
- Martin 领导 Ethane 项目
- Martin 联合 Nick教授、Scott教授创建 [Nicira](#)公司,在 SIGCOMM 会议发表论文 [《Ethance:Taking Control of the Enterprise》](#)
- The McKeown Group 发布第一个开源SDN控制器 NOX-Classic,在 SIGCOMM 会议发表论文[《OpenFlow:Enablling Innovation in Campus Networks》](#)
- 2009年，The McKeown Group 发布第一个基于Python语言的SDN控制器 POX，同时发布了 OpenFlow v1.0
- 2010年，The McKeown Group 发布开源SDN网络模拟器Mininet
- 2011年，开放网络基金会(Open Networking Foundation,ONF)成立
- 2012年，Google 发布应用SDN技术解决数据中心之间流量问题的方案
- 2012年，BigSwitch发布开源控制器 Floodlight
- 2012年，日本NTT公司发布开源控制器 Ryu

#### 成熟期：2012至今（学术界+产业界）

- 2013年，美国AT&T、英国电信、德国电信等提出了 **网络功能虚拟化（NFV）** 的概念
- 2014年，斯坦福大学、加州伯克利分校共同成立开放网络实验室（ONLab），发布开放网络操作系统（ONOS）
- 2015年
- 2016年，开放网络基金会（ONF）与开放网络实验室（ONLab）合并，主推 CORD 和 ONOS
- 2016年，思科发布全数字化网络架构-DNA
- 2017年，VMware 宣布 NSX 带来 10 亿美元销售额
- 2018年，Cisco、Apstra 等推出基于意图的网络（Intent-based Network ）

### 发展趋势

#### SD-WAN 广域网

##### 中国移动（S-Controller）

- 主控制器：中国移动（S-Controller）
- 厂商控制器：华为(D-Controller)、中兴(D-Controller)

#### SD-Security 安全

#### SD-Access 接入

#### 技术融合

基于意图的网络：机器学习+SDN控制器

### SDN Standards

系统兼容性

#### 标准化组织 ONF

开放网络基金会(Open Networking Foundation,ONF)是软件定义网络技术标准化和产业化的主要力量。

ONF 组织架构：

- 拓展组
- 配置管理组
- 测试和互操作组
- 北向接口组
- 架构组
- 混合组
- 市场组

ONF 主要工作：
- SDN 白皮书
- OpenFlow 协议
- OF-Config 协议

#### 标准化组织 IETF

因特网工程任务组（IETF）：全球互联网最具权威的技术标准化组织。SDN 技术标准主要由网络设备厂商主导。

IETF 相关研究工作组：
- 转发与控制分离组：主要关注需求、架构、协议、转发单元模型和MIB
- 应用层流量优化工作组：主要关注为应用层提供更多的网络信息，完成应用层的流量优化
- 路由领域公开的研究组：针对 I2RS 的问题描述、需求和应用场景

#### 标准化组织 ITU-T

背景：国际电信联盟
主要关注：运营商网络的场景对象和相关架构

ITU-T 关于SDN的标志性项目：
- Y.FNsdn-fmV
- Y.FNsdn

#### 标准化组织 ETSI

欧洲电信标准化协会（ETSI）

运营商发起成立网络功能虚拟化标准工作组（NFV ISG）,关注 SDN+NFV。

## 第二讲 技术架构

### SDN主流技术架构

#### 基于 OpenFlow 的三层技术架构

推动：ONF
优势：流量调度、开放生态链

####

推动：IETF
优势：开放现有网络设备的能力、标准开放的API

## 资料卡

#### 施晓秋（温州大学）

- 1985年，浙江大学物理系，理学学士
- 1985至1997年，温州大学物理教研室教师
- 1998年，浙江大学计算机系，工学硕士
- 2000年至2004年，在温州大学信息科学与工程学院，院长助理、副院长
- 2005年后，温州大学城市学院，温州大学思科网络技术学院。
- 2006年，香港科技大学访问学者
- 2007年，温州大学计算机科学与工程学院副院长。
- 2009年至2010年，温州大学物理与电子信息工程学院副院长（原物理学院与计算机学院）
- 温州大学教务处处长、教师教学发展中心主任
- 中国计算机学会教育专委会常务委员、ACM SIGCSE（中国）分会副主席

## 扩展阅读

#### SDN技术专题
- [SDN 技术指南（一）: 架构概览](https://riboseyim.com/2017/05/12/SDN/)
- [SDN 技术指南（二）：OpenFlow](https://riboseyim.com/2017/08/22/SDN-OpenFlow/)
- [SDN 技术指南（三）：SDN Controller](https://riboseyim.com/2017/10/16/SDN-Controller/)
- [SDN 技术指南（四）：Open vSwitch](https://riboseyim.com/2017/10/13/SDN-OpenvSwitch/)
- [SDN 技术指南（五）：NFV](https://riboseyim.com/2019/06/07/SDN-NFV)
- [SDN 技术指南（六）：OpenStack or Kubernetes ? ](#)
- [SDN 技术指南（七）：标准化组织](https://riboseyim.com/2019/06/07/SDN-ORG/)
- [SDN 技术指南（八）：案例教学](https://riboseyim.com/2019/06/07/SDN-CASE)
- [SDN技术指南（十）：在线课程推荐](https://riboseyim.com/2019/06/07/SDN-MOOC/)

#### 网络编程专题
- [浅谈基于数据分析的网络态势感知](https://riboseyim.github.io/2017/07/14/Network-sFlow/)
- [网络数据包的捕获与分析（libpcap、BPF及gopacket）](https://riboseyim.github.io/2017/06/16/Network-Pcap/)
- [新一代Ntopng网络流量监控—可视化和架构分析](https://riboseyim.github.io/2016/04/26/Network-Ntopng/)

#### 网络安全专题
- [Cyber-Security: IPv6 & Security](https://riboseyim.github.io/2017/08/09/Protocol-IPv6/)
- [Cyber-Security|香港拟增设网络安全与科技罪案总警司](https://riboseyim.github.io/2017/04/09/CyberSecurity-CSTCB/)

#### 云计算专题
- [AWS or Azure : 云计算平台的趋势分析|Stack Overflow,2017](https://riboseyim.com/2017/07/23/CloudComputing/)

## 参考文献
