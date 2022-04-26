---
title: SDN 技术指南（六）：OpenStack or Kubernetes ?
date: 2017-10-16 16:34:12
tags: [DevOps,网络协议,SDN]
categories: 工程技术
---
## 摘要
**待编辑**

<!--more-->

## SDN 与 OpenStack
#### OpenStack 核心组件

|组件|OpenStack实现|AWS实现|备注|
|-----|-----|-----|-----|
|计算组件（Compute）| Nova | EC2| 提供按需交付的虚拟机资源|
|镜像组件（Image）| Glance | | |
|对象存储组件（Object Storage）| Swift | S3 | 无需额外进行文件路径的挂载操作|
|块存储组件（Block Storage）| Cinder | EBS | 曾用名 Nova-volumem|
|网络服务组件（Network）| Quantum | VPC | 满足其它组件所需的网络链接要求 |
|仪表盘组件（Dashboard）| Horizon | Console| 管理操作界面|
|身份识别组件（Identity）| Keystone | IAM | 指定服务目录|

## SDN 与 Kubernetes

#### Discuss

- [Is Kubernetes Repeating OpenStack’s Mistakes? | Boris Renski	on September 7, 2017](https://www.mirantis.com/blog/is-kubernetes-repeating-openstacks-mistakes/)

- [AWS or Azure : 云计算平台的趋势分析|Stack Overflow,2017](https://riboseyim.github.io/2017/07/23/CloudComputing/)

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
- [容器化OpenStack对未来架构的影响 | 陈沙克](https://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=2653547612&idx=1&sn=849342b58c0c375992b44644e54e2599&mpshare=1&scene=1&srcid=0802Adt4W0K3y7LoOD9b3bvB#rd)
- [2016年 OpenStack 总结 | 陈沙克](http://www.chenshake.com/2016-openstack-summary/)
- [OpenStack落地的五大难点 | 肖力@云技术实践](https://mp.weixin.qq.com/s?__biz=MzAxOTAzMDEwMA==&mid=2652501209&idx=1&sn=9a22bb031b96b151af23df73f2e50452&chksm=80201de2b75794f4f67e290c0f813ce17ac4499e5f6f2f2e6fb65fa427285eb4c48296d91765&mpshare=1&scene=1&srcid=1127kiS81z5QcJhXFxtg70af#rd)
