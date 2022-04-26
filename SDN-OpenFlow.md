---
title: SDN 技术指南（二）：OpenFlow
date: 2017-08-22 09:27:32
tags: [SDN,网络协议,DevOps]
categories: 工程技术
---
## 摘要
- OpenFlow 简史（SDN != OpenFlow ; 版本问题）
- OpenFlow 工作原理
- OpenFlow is a communications protocol
- 支持 OpenFlow 的开源交换机：Open vSwitch

<!-- more -->

#### 回顾 [SDN 技术指南（一）：架构概览](https://riboseyim.github.io/2017/05/12/SDN/)
- Background：为什么需要 SDN
- SDN的主要解决方案
- SDN的整体应用架构

![](http://riboseyim-qiniu.riboseyim.com/SDN_Theme.png)

## History: OpenFlow != SDN

OpenFlow 最早由斯坦福大学提出，目前知识产权由开放网络基金会（Open Networking Foundation，ONF)持有。
SDN 和 OpenFlow 两个概念经常会被混淆和误读。这也难怪，从历史上看，两者还这是你中有我、我中有你。首先，作为一个开放的协议，OpenFlow 协议是众多 SDN 控制器解决方案的实现基础；另外，定义 SDN 概念和架构背后的许多重要人物开始在 OpenFlow 领域取得了突破，进而推动 SDN 概念走向成熟。

>OpenFlow is a key protocol in many SDN solutions.

在传统的网络交换设备中，转发平面（通常采用专门的芯片以提高性能）与控制平面（分布地部署在网络的各个节点）是紧密耦合的，被集成实现在单独的设备中。当然，从另一个角度看这样的设计也有合理性，至少能提高单个节点的灵活性和容灾能力。但是众多厂商各自为政，更出于技术保密、保持市场的考虑，对于开放接口供用户调用、建立行业标准的兴趣不大。OpenFlow 协议的推出突破了传统壁垒，有利于增加用户侧的话语权，所以 Google、Facebook 等企业是 OpenFlow 协议最坚强的拥趸，他们的数据中心都在使用 OpenFlow 协议，并倡议发起成立 ONF 来推动这个技术。

- 1995: Sun Microsystems 在1995年就提出软件定义网络的概念（在公开 JAVA 编程语言之后不久）
- 2006: [Martin Casado](https://en.wikipedia.org/wiki/Martin_Casado)(a PhD students at Stanford)及其团队首次提出了一个集中式安全控制的框架
- 2008: OpenFlow paper [《Architectural Support for Security Management in Enterprise Networks》](http://yuba.stanford.edu/~casado/mcthesis.pdf) （人物： **Nick McKeown** 、**Scott Shenker** 、**Dan Boneh**）
- 2009: Stanford 发布 OpenFlow V1.0.0 ; [Martin Casado](https://en.wikipedia.org/wiki/Martin_Casado) co-founds Nicira （主导 Open vSwitch）
- 2010: Guido Appenzeller co-founds Big Switch Networks (head of clean slate lab at Stanford)
- 2011: **Open Networking Foundation** is formed
- Oct 2011: First Open Networking Summit.
- July 2012: VMware buys Nicira for $1.26B
- Nov 6, 2013: Cisco buys Insieme for $838M

![](http://riboseyim-qiniu.riboseyim.com/SDN_Paper_OpenFlow_Page88.png)

#### OpenFlow 的版本问题

OpenFlow 协议简单来说就是把路由器的控制平面（Control Plane，管理路由表、负责网络配置和系统管理等）从转发平面（Forward Plane，转发决策和输出链路调度等）中分离出来，以软件方式实现。从第一个正式商用版本 v1.0 开始，OpenFlow 有先后推出了v1.1,v1.2,v1.3,v1.4 等，版本之间存在不兼容的内容， OpenFlow 交换机或者其它解决方案也存在版本支持不尽相同的情况，版本兼容的问题需要尤其关注。

- OpenFlow V1.0 (2009)
- OpenFlow V1.1 (2011)
**Ethernet/IP only**. **Single Flow Table**. Did not cover MPLS, Q-in-Q, ECMP, and efficient Multicast.

- OpenFlow V1.2 (2011)
**IPv6 Support**: Matching fields include IPv6 source address, destination address, protocol number, traffic class. ICMPv6 type, ICMPv6 code, IPv6 neighbor discovery header fields, and IPv6 flow labels.
TLV Matching
**Multiple controller**

- OpenFlow V1.3 (2012)
IPv6 extension headers: Can check if Hop-by-hop, Router, Fragmentation, Destination options, Authentication, Encrypted Security Payload (ESP), unknown extension headers are present
MPLS Bottom-of-Stack bit matching
MAC-in-MAC
**Multiple channels** between switch and controller

- OpenFlow V1.4 (2013)
Optical ports: Configure and monitor transmit and receive frequencies of lasers and their power
Improved Extensibility: Type-Length-Value (TLV) encodings at most places
Extended Experimenter Extension API: Can easily add ports, tables, queues, instructions, actions, etc.


## OpenFlow的工作原理

> OpenFlow is a communications protocol.

![](http://riboseyim-qiniu.riboseyim.com/SDN_OpenFlow_Workflow.png)

OpenFlow 提供了一个在 SDN 控制器和网络设备（如交换机）之间通讯的标准协议。他允许由 SDN 控制器下发到转发规则（forwarding rules）、安全规则（security rules）到底层网络交换机，完成路由决策、流量控制。OpenFlow 协议相当于一种共同语言，所以SDN 控制器和交换机都需要实现OpenFlow 协议，以便他们能够理解 OpenFlow 消息（message）。

SDN 控制器和交换机之间需要建立通讯连接才能进行配置、管理和监控。通讯连接基于 TCP （或者 TLS）协议之上，监听 6653 端口 。初始化模式：1）网络交换机发起，发送连接请求到控制器 2）控制器发起，交换机需要设置被动模式（ passive mode）开启监听。 无论使用哪种模式，一旦通讯连接建立，OpenFlow 消息将通过 TCP／TLS 连接传递。

基于 OpenFlow 消息，该协议还可以支持网络交换机监控：为了监控网络交换机，OpenFlow 协议提供了从交换机抓取网络统计信息、事件消息的请求／响应报文，方便控制器获得从交换机一侧感知人工操作和失败信息的能力,包括流移除事件，端口状态 UP/DOWN 变化等。为了能够支持第三方厂商可以在 OpenFlow 交换机上执行特定的任务，OpenFlow 协议提供可扩展的自定义消息结构，允许控制器和交换机之间传递信息。那是怎样的 OpenFlow 被许多SDN应用程序用来提供简单的网络控制和管理解决方案。

#### 流表（Flow Table）

网络交换机将 SDN 控制器下发的所有规则存储于流表（flow table），例如：

- ACL 策略（configuring ACL rules）
- 安全策略（security policy rules）
- QoS 限速策略（QoS rate limiting bandwidth rules）
- 路由策略（routing rules）
- 端口镜像策略（port mirroring rules）
- 包变更策略（packet modification rules）

![](http://riboseyim-qiniu.riboseyim.com/SDN_OpenFlow_Packet_Capture.png)
![](http://riboseyim-qiniu.riboseyim.com/SDN_OpenFlow_Packet_Capture_Mod.png)

大体上，流（flow）中包含三种类型的信息：
1. Match fields：他们将定义在包头字段：L2（源目的地 以太网地址，VLAN ID，VLAN优先级等），L3（IPv4和IPv6 源目的地 地址、协议类型、DSCP、等），L4领域（TCP/UDP/SCTP源目的端口），ARP ICMP字段，字段，MPLS域等等。
2. Actions：他们将定义一个包是否符合特定条件。例如丢弃（drop），转发到交换机的指定端口，修改数据包（push/pop VLAN ID，push/pop 标签，递增/递减IP TTL），转发到特殊端口的序列等。
3. 计数器：记录由多少数据包匹配到当前flow

OpenFlow 协议定义了多种消息来完成交换机和控制机通讯，例如：
- 连接设置消息（connection setup messages)
- 配置消息（configuration messages)
- 交换机统计信息消息（switch statistics messages)
- 连接监测消息（keep-alive messages)
- 异步事件消息（asynchronous events messages)
- 发生错误消息（error messages)


## 支持 OpenFlow 的开源交换机：Open vSwitch

> OpenFlow Switches including Open vSwitch.

市场中支持 OpenFlow 的硬件交换机包括 VMware 推出的vSwitch、vDS等虚拟交换机，Cisco与VMware合作发布了基于VMware kernel API开发的分布式虚拟交换机Nexus 1000V（功能对应于VMware的vDS），Citrix 迫于Open vSwitch快速追赶，推出了的Distributed Virtual Switch解决方案，这些方案都是收费的。

除了硬件交换机还可以通过软件支持并实现虚拟机互联，Open vSwitch(Open Source Virtual Switch)就是是一款基于软件实现的开源虚拟交换机。它采用 C 语言编写，遵循 Apache 2.0 许可。

![](http://riboseyim-qiniu.riboseyim.com/SDN_OpenvSwitch.jpg)

Open vSwitch 的周边生态情况：
- 支持的操作系统（Linux, Ubuntu, Debian，FreeBSD 和 NetBSD）
- 支持云计算平台管理系统集成，例如：OpenStack, openQRM, OpenNebula, 和 oVirt
- 支持虚拟化部署，共享硬件资源（ hypervisor ）
- 支持云平台 Xen XenServer 6.0 ，也支持 Proxmox VE, VirtualBox, Xen KVM
- 提供开发工具包：Data Plane Development Kit (DPDK)
- 接口编程语言支持：C 、Python
- 支持虚拟机通讯／监控流量统计信息，例如 NetFlow(Cisco，RFC 3954)、sFlow（RFC 3176）、NetStream（Huawei）、IPFIX（RFC 7011）,详见 [浅谈基于数据分析的网络态势感知](https://riboseyim.github.io/2017/07/14/Network-sFlow/) 。
- SPAN（Switched Port Analyzer ）, RSPAN（ Remote Switch Port Analyzer）：可以发送一份流量的拷贝给连接安全设备的交换机端口

示例：Open vSwitch 通过命令和控制器建立初始化连接（TCP），控制器 IP （192.168.56.101） 端口（6653）：
```bash
ovs-vsctl set-controller <sampleBridgeName> tcp:192.168.56.101:6653
```

Intel 拥有一个自己的 Open vSwitch 版本，OpenStack 在2011年启动 Quantum 项目，通过引入了Open vSwitch 发展 Open Stack Network 。随着 OpenStack 社区的快速壮大，Open vSwitch 在虚拟交换机的领先优势逐步确立。

## Discuss

- [资讯 | Google’s next OpenFlow challenge: taking SDNs to the consumer,Stacey Higginbotham Apr 17, 2012](https://gigaom.com/2012/04/17/googles-next-openflow-challenge-taking-sdns-to-the-consumer/)

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
- [David Mahler: Introduction to OpenFlow | Youtube](https://www.youtube.com/watch?v=l25Ukkmk6Sk)
- [TR-535 ONF SDN Evolution,VERSION 1.0,2016-09-08](https://www.opennetworking.org/wp-content/uploads/2013/05/TR-535_ONF_SDN_Evolution.pdf)
- [TS-025 ONF OpenFlow Switch Specification-Version 1.5.1 (Protocol version 0x06),March 26,2015](https://www.opennetworking.org/wp-content/uploads/2014/10/openflow-switch-v1.5.1.pdf)
- [Tarun Thakur](https://www.linkedin.com/in/tarun-thakur-36b67a19/)
- [Software Defined Networking (SDN) explained for beginners](https://www.howtoforge.com/tutorial/software-defined-networking-sdn-explained-for-beginners/)
- [Software Defined Networking (SDN) - Architecture and role of Openflow](https://www.howtoforge.com/tutorial/software-defined-networking-sdn-architecture-and-role-of-openflow/)
- [基于 Open vSwitch 的 OpenFlow 实践 | IBM@developerworks,2014](https://www.ibm.com/developerworks/cn/cloud/library/1401_zhaoyi_openswitch/)
