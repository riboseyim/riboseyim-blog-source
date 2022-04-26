---
title: SDN 技术指南（四）：Open vSwitch
date: 2017-10-13 16:37:41
tags: [SDN,网络协议,DevOps]
categories: 工程技术
---
## 摘要
- Open vSwitch 安装
- Open vSwitch 命令行
- Open vSwitch 运行原理
- Open vSwitch 性能监控

<!--more-->

#### 前言

由之前发布的文章知道 Open vSwitch(Open Source Virtual Switch) 是一款基于软件实现的开源虚拟交换机。

- [SDN 技术指南（一）: 架构概览](https://riboseyim.github.io/2017/05/12/SDN/)
- [SDN 技术指南（二）: OpenFlow ](https://riboseyim.github.io/2017/08/22/SDN-OpenFlow/)

> [Open Source Virtual Switch](http://www.openswitch.net):Community-Based, Open Source,. Full-Featured Network Operating System.

## 一、Open vSwitch 安装

```go
# 第一步：Getting the Code
git clone https://git.openswitch.net/openswitch/ops-build

# 第一步：选择编译模式

# 模式一：To a supported white box switch ( 例如 VMware vSwitch、vDS、Nexus 1000V)
make configure genericx86-64

# 模式二：OVA （可以直接导入虚拟机，例如 Oracle Virtual box ）
make configure appliance

# 第三步：打包
make

```
![](http://riboseyim-qiniu.riboseyim.com/SDN-OVS-Build.png)

![](http://riboseyim-qiniu.riboseyim.com/SDN-OVS-VM.png)

![](http://riboseyim-qiniu.riboseyim.com/SDN-OVS-Login.png)

## 二、Open vSwitch 命令行

#### 2.1 核心概念

- Bridge ：网桥，对应一个以太网交换机（Switch），一个主机中可以创建一个或者多个 Bridge 设备。
- Port ：Port 与物理交换机的端口概念类似, 每个 Port 都属于一个特定的 Bridge 。端口类型：Normal、Internal、Patch、Tunnel。
- Interface：接口，对应网卡，即可以是 ovs 生成的虚拟网卡，也可能是挂载在 ovs 的物理网卡。在通常情况下，Port 和 Interface 是一对一的关系, 只有在配置 Port 为 bond 模式后，Port 和 Interface 是一对多的关系。

#### 2.2 基本操作

- ovs-vsctl ： 查询和更新 ovs-vswitchd 的配置；
- ovs-appctl ：发送命令消息，运行相关 daemon；
- ovsdbmonitor ： GUI工具，可以远程获取 OVS 数据库和 OpenFlow 的流表。

```bash
# 创建一个新的交换机
$ ovs-vsctl add-br ovs-switch
# 创建一个端口 设置端口
# 如果在创建端口的时候没有指定 OpenFlow 端口编号，会自动生成一个
$ ovs-vsctl add-port ovs-switch p0 -- set Interface p0 ofport_request=100
# 设置接口类型
$ ovs-vsctl set Interface p0 type=internal
$ ethtool -i p0
# 查看交换机的端口信息
$ ovs-ofctl show ovs-switch
# 查看 datapath 的信息
$ ovs-dpctl show
```

## 三、Open vSwitch 运行原理

#### 3.1 Open vSwitch 内部结构

Open vSwitch 内部分为用户态和内核态。用户层（态）为守护程序实现了交换机和流表，是 Open vSwitch 的核心，提供了一些组件去管理交换机，实现数据库，对内核进行直接管理。主要包含三个守护进程：
- ovs-vswitched : 主要模块，守护进程，包括一个 Linux 内核模块。
- ovsdb-server : 数据库服务,保存相关配置信息
- ovs-brcompatd

数据流(flow) 通过 Open vSwitch 转发的流程。每收到一个包之后，OVS Kernel Module 将检查它是否能能命中内核模块的缓存（flow cache) ，如果命中缓存则交由 kernel 处理；如果不能命中缓存则先发送到用户空间（ovs-vswitchd process ）进行转发决策 ——— 基于一系列已经安装配置的规则库（OpenFlow rulues）；如果没有命中任何一条规则，则将包发送给 OpenFlow 控制器处理。一旦做出转发决策，这个包和转发动作将传回 OVS Kernel Module 缓存起来。这条 flow 接下来的包就将命中缓存并直接由 kernel 转发处理。

- openvswitch_mod.ko 是内核态(kernel)的主要模块
完成数据包的查找、转发、修改等操作，一条 flow 的后续数据包到达 OVS 后将直接交由内核态，使用 openvswitch_mod.ko 中的处理函数对数据包进行处理。

![OVS Flow Processing](http://riboseyim-qiniu.riboseyim.com/SDN-OVS-Packet-Processing.png)
![OVS包含一个Linux内核](http://riboseyim-qiniu.riboseyim.com/SDN-OVS-Command.png)

#### 3.2 Open vSwitch 的协议支持情况

- GRE-tunneled mirrors: 远程监控
- LACP、VLAN、IGMP、LLDP、BFD、STP、RSTP、QoS、HFSC
- Complete IPv6 (Internet Protocol version 6) support
- Support for multiple tunneling protocols, including GRE、VXLAN 、STT、IPsec
- Multi-table forwarding pipeline with a flow-caching engine

#### 3.3 Open vSwitch 的 OpenFlow 支持情况

- ovs-openflowd：一个简单的 OpenFlow 交换机；
- ovs-controller：一个简单的 OpenFlow 控制器；
- ovs-ofctl 查询和控制 OpenFlow 交换机和控制器；
- ovs-pki ：OpenFlow 交换机创建和管理公钥框架；
- ovs-tcpundump：tcpdump 的补丁，解析 OpenFlow 的消息；

Open vSwitch support for OpenFlow 1.1 and beyond is a work in progress.[>>> OpenFlow Support in Open vSwitch ](http://docs.openvswitch.org/en/latest/topics/openflow/)


## 四、Open vSwitch 性能监控

>“If you can’t measure it, you can’t improve it”  —— Lord Kelvin

#### 4.1 sFlow 监控示例

- 启动分析器 sFlow Analyzer (以 sFlow-RT 为例)

```go
$ cd sflow-rt
$ ./start.sh
bash-3.2$ ./start.sh
信息: Listening, sFlow port 6343
信息: Listening, HTTP port 8008
信息: app/ovs/scripts/status.js started
警告: app/ovs/scripts/status.js app/ovs/scripts/status.js
信息: app/ovs/scripts/status.js stopped
$ ps -ef | grep 6343
  501 30565 30431   0  2:45下午 ttys002    0:03.90 /usr/bin/java -Xms200M -Xmx200M -XX:+UseG1GC -XX:MaxGCPauseMillis=100 -Dsflow.port=6343 -Dhttp.port=8008 -jar ./lib/sflowrt.jar
```

- Connect Normal Switch to sFlow Analyzer

```go
// 指定 analyzer
switch(root)# sflow collector 10.0.0.1
// 数据包采样: 1-in-4096
// 常规参考值：[100 Mb/s: 1 in 500]、[1 Bb/s: 1 in 1000]、[10 Gb/s: 1 in 2000]
switch(root)# sflow sampling 4096
// 轮询计数器 polling counters every 20 seconds
switch(root)# sflow polling 20
switch(root)# sflow enable
```

-  Connect Open vSwitch to sFlow Analyzer

```go
// e.g. connect Open vSwitch to sFlow analyzer
ovs-vsctl — –id=@sflow create sflow agent=eth0 \target=\”10.0.0.1:6343\” sampling=1000 polling=20 \
— set bridge br0 sflow=@sflow
```

#### 4.2 Connect Open vSwitch to OpenFlow controller

```go
// e.g. connect Open vSwitch to OpenFlow controller
ovs-vsctl set-controller br0 tcp:10.0.0.1:6633
```

#### 4.3 Traffic analytics : sFlow vs NetFlow

- sFlow does not use flow cache, so realtime charts more accurately reflect traffic trend
- NetFlow spikes caused by flow cache active-timeout for long running connections

![](http://riboseyim-qiniu.riboseyim.com/SDV-OVS-sFlow-NetFlow.png)


#### 应用案例
- [传统券商的互联网技术之路——泛前端、交易云与金融电商](https://mp.weixin.qq.com/s?__biz=MzAxNDU2MTU5MA==&amp;mid=208243964&amp;idx=1&amp;sn=161fdc0288aa36a93ca34d488f321b88&amp;scene=1#rd)

## News

#### Open vSwitch: Enable OpenFlow 1.5 by default | 25 Apr 2017

```js
===================== ===== ===== ===== ===== ===== =====
Open vSwitch          OF1.0 OF1.1 OF1.2 OF1.3 OF1.4 OF1.5
===================== ===== ===== ===== ===== ===== =====
1.9 and earlier        yes   ---   ---   ---   ---   ---
1.10, 1.11             yes   ---   (*)   (*)   ---   ---
2.0, 2.1               yes   (*)   (*)   (*)   ---   ---
2.2                    yes   (*)   (*)   (*)   (%)   (*)
2.3, 2.4               yes   yes   yes   yes   (*)   (*)
2.5, 2.6, 2.7          yes   yes   yes   yes   (*)   (*)
2.8, 2.9, 2.10, 2.11   yes   yes   yes   yes   yes   (*)
2.12                   yes   yes   yes   yes   yes   yes
===================== ===== ===== ===== ===== ===== =====
```

## 前沿

- [Open vSwitch: Overview of 802.1ad (QinQ) Support](https://developers.redhat.com/blog/2017/06/06/open-vswitch-overview-of-802-1ad-qinq-support/)
- [Open vSwitch: QinQ Performance](https://developers.redhat.com/blog/2017/06/27/open-vswitch-qinq-performance/)

## 扩展阅读

#### SDN技术专题
- [SDN 技术指南（一）: 架构概览](https://riboseyim.com/2017/05/12/SDN/)
- [SDN 技术指南（二）：OpenFlow](https://riboseyim.com/2017/08/22/SDN-OpenFlow/)
- Preview [SDN 技术指南（三）：SDN Controller](https://riboseyim.com/2017/10/16/SDN-Controller/)
- [SDN 技术指南（四）：Open vSwitch](https://riboseyim.com/2017/10/13/SDN-OpenvSwitch/)
- Preview [SDN 技术指南（五）：NFV](https://riboseyim.com/2019/06/07/SDN-NFV)
- Preview [SDN 技术指南（六）：OpenStack or Kubernetes ? ](#)
- Preview [SDN 技术指南（七）：标准化组织](#)
- Preview [SDN 技术指南（八）：案例教学](#)
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
- [An Introduction to OVS Architecture | YY哥](http://hustcat.github.io/an-introduction-to-ovs-architecture/)
- [OVS 流表原理 | YY哥](http://hustcat.github.io/ovs-flow-table-in-datapath/)
- [kontrolissues:Installing OpenSwitch](https://kontrolissues.net/2015/10/21/installing-openswitch/)
- [OVS初级教程：使用Open vSwitch构建虚拟网络](http://www.sdnlab.com/sdn-guide/14747.html)
- [基于 Open vSwitch 的 OpenFlow 实践 | IBM@developerworks,2014](https://www.ibm.com/developerworks/cn/cloud/library/1401_zhaoyi_openswitch/)
- [Offloading OVS Flow Processing using eBPF | William (Cheng-Chun) Tu VMware | OVS Conference 2016](http://openvswitch.org/support/ovscon2016/7/1120-tu.pdf)
- [Traffic visibility and control with sFlow | Peter Phaal InMon Corp. November 2014](http://openvswitch.org/support/ovscon2014/17/1400-ovs-sflow.pdf)
- [Accelerating Open vSwitch to “Ludicrous Speed” | Network Heresy , 2014 ](https://networkheresy.com/2014/11/13/accelerating-open-vswitch-to-ludicrous-speed/)
- [Open Virtual Network (OVN) | sflow.com ,2015 ](http://blog.sflow.com/2015/09/open-virtual-network-ovn.html)
- [Open vSwitch 2014 Fall Conference ](http://openvswitch.org/support/ovscon2014/)
- [Open vSwitch 2015 Fall Conference ](http://openvswitch.org/support/ovscon2015/)
- [Open vSwitch 2016 Fall Conference ](http://openvswitch.org/support/ovscon2015/)
