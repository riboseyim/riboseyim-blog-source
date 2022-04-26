---
title: SDN技术指南：Open Network Operating System
date: 2017-08-18 12:12:06
tags: [SDN,DevOps,DevOps]
categories: 工程技术
---
## 摘要
- Samsung 主办 ONOS Build 2017
- Google 加入CORD
- ONOS vs. OpenDaylight

<!-- more -->

> ONOS source code, written in Java

## ONOS Architecture

![](http://riboseyim-qiniu.riboseyim.com/SDN-CORD-architecture.png)

## Samsung 主办 ONOS Build 2017

[原文：Samsung Hosts ONOS Build 2017 and Fuels SDN Innovation]( https://www.linux.com/blog/onos/2017/8/samsung-hosts-onos-build-2017-and-fuels-sdn-innovation)

三星Samsung将于 9月 20-22 在韩国首尔的研发中心主办 ONOS Build 2017 。

![](http://riboseyim-qiniu.riboseyim.com/small-palace-tower-landmark-place-of-worship-temple.jpg)

开放网络操作系统(ONOS , Open Network Operating System) 社区在全世界各个地区快速发展，ONOS Build 2017 是开放网络基金会（ONF，Open Networking Foundation）组织的大型开发者大会，专注于 ONOS 项目的研发与推广工作，将于 2017 年 9 月 20 日- 22日在韩国首尔的三星研发中心举办。

本次年度活动将联合200多名开发商和贡献者一起分享，学习，调整，规划，和PK。大会将提供 ONOS 专家主题演讲，人们可以通过社区展示活动了解相关的工作信息，这将是一次演示汇报的 SDN 科技博览会 以及一次编程马拉松活动。我们也将了解到三星Samsung，作为开放网络基金会的合作伙伴之一，为什么投资 ONOS 建设。

####  为什么三星投资主办 ONOS 大会 ?
三星认为，通过开源社区的努力，ONS 项目作为组织的核心将铺平道路、加速革新。作为一家领先的网络解决方案提供商，三星很高兴能帮助那些正在推动创新并将SDN技术引入全球电信网络的开发人员。 

#### 什么三星投资 ONOS 项目 ?
ONOS 的目标是通过 SDN 的方式达到运营商级的性能指标，包括服务保证，可靠性和可扩展性和高性能。三星相信 ONOS 项目在实现传统网络转向灵活和可扩展系统方面以及走在前列，将使运营商能够更有效地运行他们的网络，为即将到来的5G 时代做好准备。
三星一直在积极地促成和加速塑造下一代网络服务，基于 ONOS 项目的开源 SDN 和网络虚拟化。我们相信三星的贡献将有助于满足运营商在电信领域提供领先的5G服务的要求。

#### 三星参与 ONOS 项目多久了 ?
三星在 2014 年基于现成的 ONOS 项目开发了首个商业的 SDN 产品，并且在 2016年作为董事会成员加入 ONOS 项目。自从加入该项目，三星一直积极促进每个版本的发展，和其他运营商密切合作开发商业级的 SDN 解决方案。借助于大量具备丰富的 SDN 工作经验开发者、对电信网络整体架构的深刻理解，三星正在社区扮演一个社区规范主要贡献者的角色，有助于在市场中提升相关技术可用性。

#### ONOS Build 愿景
ONOS 大会是一个年度会议，将在世界不同地区举行，连接来自不同背景和行业的全球开发者。今年，我们邀请创新者到亚洲为亚太地区的学术和商业生态系统注入新的活力。此外，该活动将成为开发人员在SDN技术及应用中促进和分享其成果的平台。与会者通过年度共享，参与公开讨论，将增加创新 SDN 商业解决方案的水平，有助于SDN发展。最后，全球许多运营商将出席 ONOS 大会。通过连接运营商，我们希望我们能与开发者分享 SDN 的愿景和技术进步。这将极大地改变行业，帮助我们走向更大的网络的可能性。

#### ONOS 2017 议程
ONOS 2016 大会是首次建立一个强大的 SDN 技术基础的活动。近一年以来，有越来越多的移动运营商愿意将技术融入他们的网络。 ONOS  2017 大会将在电信行业参与者感知到快速变化的背景下召开的，将推动全球电信市场发展商业化的运营商级 SDN 。从内容上讲，今年的活动将持续3天，为与会者提供额外一天的对话会议，让他们深入了解彼此的技术并展示工作成果。最后一天，还将会有旨在介绍 CORD （Central Office Re-Architected as a Data Center）项目的相关进展。

#### [ONOS Build 2017 Schedule](http://onosbuild.org/schedule/)

## 相关资讯

#### Google 加入CORD
2016年7月26日，CORD 项目宣布成为Linux基金会下的一个独立的开源项目，Google、Radisys以及三星电子宣布加入该项目。CORD的愿景是为规模化的数据中心服务，CORD 创建了两个控制平面：一个使用ONOS控制器，负责网络服务；另外一个使用新的 CORD 操作系统 — XOS，管理VNFs，该操作系统将一切都看作服务。

#### ONOS vs. OpenDaylight
ONOS  的第一个版本由 Open Networking Lab (ON.Lab)  在2014年贡献到社区（源码用 Java 编写），2015年10月, ONOS 项目 作为 合作项目（collaborative） 加入 Linux Foundation。 OpenDaylight (ODL)  由 Linux Foundation 创建。两者在推动 SDN 的设计和目标上非常相似，主要是支持者和合作伙伴不同。ONOS 更倾向于网络运营商，更关注提供更好的整体网络性能；ODL更倾向于数据中心网络，设计上注重融合传统网络与 SDN 。

- [What is the OpenDaylight Project (ODL)?](https://www.sdxcentral.com/sdn/definitions/opendaylight-project/)


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
- [AWS or Azure : 云计算平台的趋势分析|Stack Overflow,2017](https://riboseyim.github.io/2017/07/23/CloudComputing/)


## 参考文献
- [Wiki: ONOS](https://en.wikipedia.org/wiki/ONOS)
- [OpenCORD.org](http://opencord.org/)
- [sdnlab.com: AT&T CORD架构解读](http://www.sdnlab.com/17777.html)
- [sdnlab.com: Google加入CORD](http://www.sdnlab.com/17495.html)
- [OpenNetworking.org: ONF Overview](https://opennetworking.org/about/onf-overview)
- [techtarget.com: ONOS (Open Network Operating System)](http://searchsdn.techtarget.com/definition/ONOS-Open-Network-Operating-System)
