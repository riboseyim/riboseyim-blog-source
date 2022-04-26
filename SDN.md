---
title: SDN 技术指南（一）：架构概览
date: 2017-05-12 15:54:48
tags: [SDN,网络协议,DevOps]
categories: 工程技术
---
## 摘要
- Background：为什么需要 SDN
- SDN的主要解决方案
- SDN的整体应用架构
- SDN与网络安全
- OpenFlow工作原理
- OpenFlow在SDN架构中的角色

<!-- more -->

## Background
软件定义网络（Software-defined networking，SDN），一种新的网络架构。SDN 提出的控制与转发平面分离、网络状态集中控制、支持软件编程等理念并不是什么新鲜事，但是长久以来一直没有非常突破性的进展。

>“为了让系统更好地工作，早期需要管理复杂性而后期需要提取简单性。” —唐·诺曼（Donald Arthur Norman）

目前 SDN 引起广泛关注得益于网络需求侧翻天覆地的变化：云计算业务（服务器虚拟化技术为代表）成为主流，移动互联网催生的大数据技术日益普及，包括网络在内的资源快速配置、弹性扩容、按需调用需求强烈。传统模式的弊端显现：网络设备硬件、操作系统和网络应用三部分紧耦合在一起，组成一个封闭系统，这三部分相互依赖、每一部分的创新和演进都要求其余部分做出同样的升级。

越来越多的网络新协议和新算法使得网络控制平面变得越来越复杂，但是现在的网络用户却对网络的易用性有更高的要求，希望网络具有更多的可编程能力，从而自动化、智能化网络管理。正如 SDN 的倡导者 [Scott Shenker,U.C. Berkeley Professor](https://en.wikipedia.org/wiki/Scott_Shenker) 所言，网络发展目前还处于“管理复杂性”阶段，这样的架构严重阻碍了网络创新进程的开展。

## SDN Solutions

如何解决从“管理复杂性”阶段转变到“提取简单性”阶段呢？最先取得成功商用经验的是互联网企业Google。

#### Google 的 SDN 实践

Google 基于 SDN 技术改造其骨干网 G-scale（Backbone Network，也称WAN网）。WAN网的主要任务是负责全球12个数据中心之间的互联，数据流量的内容包括：1. 用户数据备份，例如视频、图片、语音等；2. 跨数据中心存储访问，例如计算资源和存储资源分布不同；3. 大规模的数据同步。WAN 网成本高昂（包括很多海底光缆），而且存在数据流量大但是链路带宽利用率低的问题：为了实现负载均衡，同时避免大流量都被分发到同一个链路上导致丢包，Google不得不使用过量链路，提供比实际需要多得多的带宽，实际链路带宽利用率只有30%~40%，而且仍不可避免有的链路很空闲，有的链路产生拥塞，设备必须支持很大的包缓存，成本高。为了增强网络的可管理性，Google 首先在带宽分配和路径计算方面尝试。解决思路是当一个新的数据要开始传输时，应用程序会评估所需要耗用的带宽，为它选择一条最优路径（如负载最轻但非最短路径，虽不丢包但延时大），然后把这个应用对应的策略通过控制器（Controller）下发到定制的交换机中，跟选择的路径绑定在一起，从而整体上使链路带宽利用率达到最优。

SDN 架构中最显著的一个特点就是采用集中式控制器（Controller）：
![](http://riboseyim-qiniu.riboseyim.com/SDN_Solutions_201708.png)


## SDN Architecture

SDN在应用中大体上可以可以划分为三层体系结构：
- 应用层（Application Layer）
- 控制层（Control Layer）
- 基础设施层（Infrastructure Layer）

不同层次之间通过接口通讯：
- 北向接口（Northbound interface）
- 南向接口（Sorthbound interface）

![](http://riboseyim-qiniu.riboseyim.com/SDN_Arch_OpenFlow_201708.png)

#### 控制层（ Control Layer ）

控制层是 SDN 控制器管理网络的基础设施，可以根据需要灵活选择多种控制器。
在这一层中，控制器中包含大量业务逻辑，以获取和维护不同类型的网络信息、状态详细信息、拓扑细节、统计详细信息等。
由于 SDN 控制器是用于管理网络的，所以它必须具有用于现实世界网络使用情况的控制逻辑，如交换、路由、二层VPN、三层VPN、防火墙安全规则、DNS、DHCP和集群，网络供应商和开源社区需要在自己的 SDN 控制器中实现自己的服务。这些服务会向上层（应用层）公开自己的API（通常是基于 [REST](https://riboseyim.github.io/2017/05/23/RestfulAPI/) ，这使网络管理员可以方便地使用应用程序上的 SDN 控制器的配置、管理和监控网络。

目前市场上的 SDN 控制器解决方案大致可以分为两类：大型网络设备厂商提供商业方案，例如 Cisco Open SDN controller, Juniper Contrail, Brocade SDN controller, 和来自 NEC 公司的 PFC SDN controller ；社区组织提供的开源方案，例如 OpenDaylight, Floodlight, Beacon, Ryu 等等。

Commercial Solutions:
- [Cisco Open SDN Controller](https://www.cisco.com/c/en/us/products/cloud-systems-management/open-sdn-controller/index.html)
- [Juniper Contrail](https://www.juniper.net/us/en/products-services/sdn/contrail/)
- [Brocade SDN controller](http://www.luminanetworks.com/products/?utm_source=brocade&utm_medium=redirect&utm_campaign=launch)
- [PFC SDN controller(From NEC)](https://www.necam.com/sdn/Software/SDNController/)

Open Source Solutions:
- [Beacon](https://www.sdxcentral.com/projects/beacon/)：由斯坦福大学开发，Java语言编写
- [Floodlight](http://www.projectfloodlight.org/floodlight/)：源于[Beacon](https://www.sdxcentral.com/projects/beacon/)，Big Switch Networks开发，Java语言编写，Apache许可证
- [OpenDaylight](https://www.opendaylight.org/)：
- [Ryu](http://osrg.github.io/ryu/)：由 NTT 开发，Python 编写，能够与 OpenStack 平台整合，控制器API丰富
- [Mul](#): 由 Kulcloud 开发，内核采用 C 语言实现的多线程架构
- [NodeFlow](#): 由 Cisco 开发，基于 Node.js 的 [OpenFlow](https://riboseyim.github.io/2017/08/22/SDN-OpenFlow/) 控制器，JavaScript 编写
- [Trema](#): 由 NEC 开发，Ruby/C 编写
- [NOX](#): 由 Nicira 开发，C++/Python编写，业界第一款 [OpenFlow](https://riboseyim.github.io/2017/08/22/SDN-OpenFlow/) 控制器
- [POX](#): 由 Nicira 开发，是 NOX 的纯 Python 实现版本，目的是提供跨平台部署的便利性


#### 基础设施层（ Infrastructure Layer ）
基础设施层，由各种网络设备构成。它可以是数据中心的一组网络交换机和路由器。控制层负责管理底层物理网络，物理层的实现可以是支持 OpenFlow 的硬件交换机，随着虚拟化技术的完善，SDN 交换机可以是软件形态，例如 [Open vSwitch (OVS)](http://openvswitch.org) 就是一款基于开源技术实现的、能够与服务器虚拟化（Hypervisor）集成，具备交换机的功能，可以实现虚拟化组网。另外，OVS 支持传统的标准管理接口，例如 NetFlow 、sFlow 等，监测虚拟环境中的流量情况，详见 [《浅谈基于数据分析的网络态势感知》](https://riboseyim.github.io/2017/07/14/Network-sFlow/) 。

#### 应用层（ Application Layer ）

应用层对于开发者来说是开放区域，鼓励开发尽可能多的创新应用。包括网络的可视化：拓扑结构、网络状态、网络统计等；网络自动化相关应用：网络配置管理，网络监控，网络故障排除，网络安全策略等。SDN 应用程序可以为企业和数据中心网络提供各种端到端的解决方案。

例如，Brocade 应用实例：
- Brocade Flow Optimizer
- Brocade Virtual router
- Brocade Network advisor

HPE 应用实例：
- HPE Network Optimizer
- HPE Network protector
- HPE Network visualizer
- NEC UNC for HP SDN VAN Controller
- Aricent SDN Load balancer
- TechM smart flow steering
- TechM server load balancer

#### 南向接口（ Southbound interface ）
控制层到基础设施层（网络交换机）通讯需要经过南向接口，目前主要的协议是 OpenFlow , NetConf，OVSDB 。 OpenFlow 协议是事实上的国际行业标准，NOX 、Onix 、Floodlight 等都是基于 OpenFlow 控制协议的开源控制器。作为一个开放的协议，OpenFlow 突破了传统网络设备厂商各自为政形成的设备能力接口壁垒。

#### 北向接口（ Northbound interface ）
北向接口：应用层 通过 API 的方式 与 SDN 控制器通讯。与南向接口不同，现在北向接口还缺少业界公认的标准，实现方案思路有的从用户角度出发、有的从运营商角度出发、有的从产品能力角度出发。技术风格上，部分传统的网络设备厂商倾向于在现有的设备上提供编程接口供业务App调用，许多上层应用的开发者也比较倾向于采用 [REST API](https://riboseyim.github.io/2017/05/23/RestfulAPI/) 接口的形式。

#### SDN 世界的两大山头

SDN 技术体系目前还处于激烈竞争阶段，相关新产品和新技术层出不穷，如果要梳理大致可以分为两个流派：

- ONF(Open Networking Foundation，开放网络基金会 )
  董事会成员：德国电信（DT）、Facebook、 Google, Microsoft、Verizon、Yahoo!、日本 NTT 电信、高盛公司
  特点：面向用户

- 传统巨头大联盟（通过Linux基金会(Linux Foundation)合作）
  成员：思科（Cisco）、IBM、 微软、Big Switch、博科、思杰、戴尔、爱立信、富士通、英特尔、瞻博网络、微软、NEC、惠普、红帽和VMware
  协作项目：[OpenDaylight（20130408）](https://www.sdxcentral.com/sdn/definitions/opendaylight-project/)
  特点：大厂控制“嫌疑”

#### SDN 标准化组织
- IETF（Internet Engineering Task Force，互联网工程任务组）
  相对 ONF 而言，更多是由网络设备厂商主导，已经发布了多篇 [RFC](https://riboseyim.github.io/2017/05/12/Protocol-RFC/) 文稿，内容涉及需求、框架、协议、转发但愿模型及 MIB 等。

- ETSI NFV(Network Functions Virtualisation)
  成员：欧洲电信标准协会（European Telecommunications Standards Institute，ETSI）包括 AT&T, 英国电信（BT）, 德国电信等
  特点：主要工作成果是 “网络功能虚拟化白皮书”，对NFV的定义、应用场景、基本功能，以及SDN等技术的关系等内容进行描述。

- ITU-T （国际电信联盟通信标准化组织）
  由 ITU-T 指定的国际标准通常被称为建议（Recommendations）,2012年开始 SDN 与电信网络结合的标准研究。

![](http://riboseyim-qiniu.riboseyim.com/SDN_Ecosystem.png)

## SDN 与网络安全 （待补充）

#### 高可用性支持

- 如果 SDN 控制器只配置了一条源和目的路径，一旦链路失效如何快速将流量路由到正常链路？（持续监控网络拓扑、实时性）
- 外部连接的高可用性，支持VRRP（Virtual Router Redundancy Protocol）, MC-LAG(Multi-Chassis Link Aggregation Group)等
- 如何避免控制器成为新的单点故障源？（硬件、软件冗余设计，控制器集群化（OpenFlow v1.2开始引入），控制器集群同步能力）

#### 企业级的授权和隔离

- 支持企业级的授权和认证
- 确保租户能在共享的网络基础资源上获得完全隔离的虚拟网络

#### 抗恶意攻击能力
- 需要具备对控制类通讯的流量限制，怀疑被攻击是的告警能力


## Discuss
- [云数据中心网络分析及安全技术方案实践(DPDK是一组快速处理数据包的开发平台及接口) | 王凯@云杉网络研发部 ](http://www.infoq.com/cn/articles/cloud-data-center-network-and-security-technology-solutions?utm_campaign=infoq_content&utm_source=infoq&utm_medium=feed&utm_term=global)
- [走近Google基于SDN的B4网络 | @盛科张卫峰](http://www.csdn.net/article/2013-11-25/2817613)
- [863项目：未来网络体系结构和创新环境(FINE) | @清华大学-毕军](http://www.jifang360.com/news/2012128/n966142974.html)
- [百度：如何优化多数据中心的带宽成本？](https://mp.weixin.qq.com/s?__biz=MzA4Nzg5Nzc5OA==&amp;mid=400077810&amp;idx=1&amp;sn=95f4da1404d23aaa8d977a7b52dcc2d3&amp;scene=1&amp;srcid=1023P4qUPOLAD2K9sN9qyLFp#rd)


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

- [资讯：三星主办 ONOS Build 2017](https://riboseyim.github.io/2017/08/18/SDN-ONOS/)
- [What is the OpenDaylight Project (ODL)?](https://www.sdxcentral.com/sdn/definitions/opendaylight-project/)
- [对话大神Scott Shenker：从物理博士到SDN's Uncle | CSDN,2014](http://www.csdn.net/article/2014-06-05/2820097-Scott-Shenker)
- [新贵与遗老：被集群路由器和POS绑架的运营商网络 | 徐建锋](https://mp.weixin.qq.com/s?__biz=MjM5MzEzMTY0MA==&amp;mid=200439739&amp;idx=2&amp;sn=2e0dcb987bb6b093f57a0ecac1c86210&amp;scene=0#rd)
- [一张复杂网络的生长过程| jaseywang.me](http://jaseywang.me/2017/02/27/%e4%b8%80%e5%bc%a0%e5%a4%8d%e6%9d%82%e7%bd%91%e7%bb%9c%e7%9a%84%e7%94%9f%e9%95%bf%e8%bf%87%e7%a8%8b/)
