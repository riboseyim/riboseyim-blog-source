---
title: How Linux Works:Router
date: 2019-03-05 11:00:55
tags: [SRE,Linux,DevOps,架构师]
categories: 工程技术
---
## 摘要
- 编辑中
- 路由协议
- router based linux kernel

<!--more-->

## BASIC

#### BASIC Commands

```sh
route -n
# To Host
route add -host [ip] dev eth0
route add -host [ip] gw [ip]
# To Network
route add -net [ip] netmask [mask]
route del -net [ip] netmask [mask] gw [......]
# default route
route add default gw [......]
```

## OpenSource Routing Projects

#### What is Quagga?

Quagga is a routing software suite, providing implementations of OSPFv2, OSPFv3, RIP v1 and v2, RIPng and BGP-4 for Unix platforms, particularly FreeBSD, Linux, Solaris and NetBSD. Quagga is a fork of GNU Zebra which was developed by Kunihiro Ishiguro.

The Quagga architecture consists of a core daemon, zebra, which acts as an abstraction layer to the underlying Unix kernel and presents the Zserv API over a Unix or TCP stream to Quagga clients. It is these Zserv clients which typically implement a routing protocol and communicate routing updates to the zebra daemon. Existing Zserv implementations are:  

| IPv4  | IPv6   | desc                                                             |
| ----- | ------ | ---------------------------------------------------------------- |
| zebra | zebra  | kernel interface, static routes, zserv server                    |
| ripd  | ripngd | RIPv1/RIPv2 for IPv4 and RIPng for IPv6                          |
| ospfd | ospf6d | OSPFv2 and OSPFv3                                                |
| bgpd  | bgpd   | BGPv4+ (including address family support for multicast and IPv6) |
| isisd | isisd  | IS-IS with support for IPv4 and IPv6                             |
| vtysh | vtysh  | shell tools              

#### Zebra

Zebra 是一个 TPC/IP 路由软件，支持 BGP、OSPF、RIP 和 RIPng。它遵循 GNU 通用公共许可协议，可以运行于 Linux 操作系统上（在 Red Hat 中已经附带了 Zebra 的 RPM 安装包）。

最初的 Zebra 软件包由 Kunihiro Ishiguro 和 Yoshinari Yoshikawa 于1996年完成。现在主要由 IP Infusion 及开源志愿者维持。

Zebra 采用模块的方法来管理协议。可以根据网络需要启用或者禁用协议。此外，Zebra 的配置同 Cisco IOS 极其类似，这一点对于已经熟悉 Cisco 的网络工程师来说非常方便。

[Zebra](http://www.zebra.org/) is a multi-server routing software which provides TCP/IP based routing protocols. Zebra turns your machine into a full powered router. Some of the features of Zebra include:

- Common routing protocols such as RIP, OSPF, BGP supported.
- IPv6 routing protocols such as RIPng and BGP-4+ supported.
- User can dynamically change configuration from terminal interface.
- User can use command line completion and history in terminal interface.
- IP address based filtering, AS path based filtering, attribute modification by route map are supported.

#### Supported Platforms

**操作系统支持:**

- GNU/Linux
- FreeBSD/NetBSD/OpenBSD

**IPv6 支持:**

- NRL IPv6
- KAME
- INRIA IPv6

#### Quagga ABC

[Quagga](https://www.quagga.net/) daemons are each configurable via a network accessible CLI (called a 'vty'). The CLI follows a style similar to that of other routing software. There is an additional tool included with Quagga called 'vtysh', which acts as a single cohesive front-end to all the daemons, allowing one to administer nearly all aspects of the various Quagga daemons in one place.

```bash
# The default location

/usr/local/sbin/zebra

/usr/local/etc/zebra.conf
```

## 下载

- [Quagga](http://download.savannah.gnu.org/releases/quagga/)
- The [BIRD Internet Routing Daemon Project](http://bird.network.cz/), supported by cz.nic.
- The [OpenBGPd and OpenOSPFd project](http://www.openbgpd.org/), primarily developed for OpenBSD.

## 扩展阅读

#### 电子书《Linux Perf Master》

- https://riboseyim.gitbook.io/perf
- https://www.gitbook.com/book/riboseyim/linux-perf-master/details

![](http://riboseyim-qiniu.riboseyim.com/banner-LPM-201803.png)

#### 性能诊断指南
- [Linux 性能诊断：负载评估](https://riboseyim.com/2017/12/11/Linux-Perf-Load/)
- [Linux 性能诊断：快速检查单](https://riboseyim.com/2017/12/11/Linux-Perf-Netflix/)
- [Linux 性能诊断：JVM](https://riboseyim.com/2018/08/07/Linux-Perf-JVM/)

#### How Linux Works
- [How Linux Works：The Big Picture](https://riboseyim.com/2019/04/21/Linux-Works/)
- [How Linux Works：BASIC Commands](https://riboseyim.com/2017/04/26/Linux-Commands/)
- [How Linux Works：BASIC Commands Extension](https://riboseyim.com/2018/09/03/Linux-Commands-New/)
- [How Linux Works：Device and FileSystem](https://riboseyim.com/2018/06/07/Linux-Works-FileSystem/)
- [How Linux Works：Boots](https://riboseyim.com/2017/05/29/Linux-Works-Boots/)
- [How Linux Works：用户空间](https://riboseyim.com/2019/04/21/Linux-Works-User-Space/)
- [How Linux Works：内存管理](https://riboseyim.com/2017/12/11/Linux-Works-Memory/)
- [How Linux Works：网络管理](https://riboseyim.com/2018/01/08/Linux-Works-Network/)
- Preview[How Linux Works：路由管理](https://riboseyim.com/2019/03/05/Linux-Works-Router/)

#### 动态追踪技术
- [动态追踪技术(一)：DTrace 导论](https://riboseyim.com/2016/11/26/DTrace/)
- [动态追踪技术(二)：strace+gdb 溯源 Nginx 内存溢出异常 ](https://mp.weixin.qq.com/s?__biz=MjM5MTY1MjQ3Nw==&mid=2651939588&idx=1&sn=35f71c5f88d1edf23cb2efc812ab8e6c&chksm=bd578c168a20050041c08618281691f0111f61c789097a69095933057618637fc54817815921#rd)
- [动态追踪技术(三)：Tracing Your Kernel Function!](https://riboseyim.com/2017/04/17/DTrace_FTrace/)
- [动态追踪技术(四)：基于 Linux bcc/BPF 实现 Go 程序动态追踪](https://riboseyim.com/2017/06/27/DTrace_bcc/)
- [动态追踪技术(五)：Welcome DTrace for Linux](https://riboseyim.com/2018/02/16/DTrace-Linux/)

#### 案例与实务
- [最佳工程实践：Stack Overflow 架构 - 2016 Edition](https://riboseyim.github.io/2016/07/17/OpenSource-StackOverflow/)
- [最佳工程实践：Oracle 数据库迁移割接实践](https://riboseyim.github.io/2016/06/12/Technology-Oracle/)
- [最佳工程实践：基于LVS的AAA负载均衡架构实践](https://riboseyim.github.io/2016/09/01/AAA/)  
- [VIPServer | Facebook Open-sourcing Katran, a scalable network load balancer](https://code.facebook.com/posts/1906146702752923/open-sourcing-katran-a-scalable-network-load-balancer/)

## 参考文献
- [在 Linux 上构建网络路由器 | Dominique Cimafranca 和 Rex Young | 2004 年 3 月 09 日发布](https://www.ibm.com/developerworks/cn/linux/l-emu/index.html)
- [Linux下使用Quagga(Zebra)搭建路由器记录](https://www.cnblogs.com/sanyuanempire/articles/6155254.html)
- [How to Make a CentOS 7 Router](https://linuxhint.com/centos7_router/)
