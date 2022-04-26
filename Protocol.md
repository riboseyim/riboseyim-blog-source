---
title: 计算机网络协议 | Overview of Protocol
date: 2017-05-26 10:30:59
tags: [网络协议,DevOps]
categories: 工程技术
---
## 摘要

<!--more-->

## 一、网络协议大全

#### 1.1 [TCP](https://riboseyim.github.io/2017/05/12/Protocol-TCP/)
- Transmission Control Protocol (TCP)  RFC 793
#### 1.2 [UDP](https://riboseyim.github.io/2017/05/12/Protocol-UDP/)
- User Datagram Protocol (UDP)  RFC 768
#### 1.3 IP
- [计算机网络协议：IPv6 与网络安全](https://riboseyim.github.io/2017/08/09/Protocol-IPv6/)
- IPv6 Routing Header RFC 1883
- IPv6 Fragment Header RFC 1883
- ICMP for IPv6 RFC 1883
- VPC/RFB Protocol：绿盟安全审计系统 [CN102821161B](https://patents.google.com/patent/CN102821161B)

- **Star** [Let's code a TCP/IP stack](http://www.saminiir.com/lets-code-tcp-ip-stack-1-ethernet-arp/#tuntap-devices)

#### 1.4 Link Layer

链路层发现协议（Link Layer Discovery Protocol，LLDP）是一种数据链路层协议，网络设备可以通过在本地网络中发送LLDPDU（Link Layer Discovery Protocol Data Unit）来通告其他设备自身的状态。是一种能够使网络中的设备互相发现并通告状态、交互信息的协议。

- [802.1AB Overview | Link Layer Discovery Protocol | IEEE 802.3 Frame Expansion Study Group
Ottawa | Sept 30, 2004](http://www.ieee802.org/3/frame_study/0409/blatherwick_1_0409.pdf)
- [配置链路层发现协议(LLDP)在交换机的端口设置 | Cisco](https://www.cisco.com/c/zh_cn/support/docs/smb/switches/cisco-250-series-smart-switches/smb2767-configure-link-layer-discovery-protocol-lldp-port-settings-o.html)  

#### 其它
- Internet Control Messages Protocol (ICMP)  RFC 792
- Internet Group Management Protocol (IGMP)  RFC 3376

- Exterior Gateway Protocol  RFC 888

#### 2.1 Software
- [计算机远程通信协议：从 从 CORBA 到 gRPC](https://riboseyim.github.io/2017/10/30/Protocol-gRPC/)
- DNSSEC : Domain Name System Security Extensions ，CDNS安全扩展，RFC 2535


## 二、调试工具

#### 2.1 [关于网络数据包的捕获、过滤和分析(Packet Capturing)](https://riboseyim.github.io/2017/06/16/Network-Pcap/)
- What is Packet Capturing
- How can it be used
- What is libpcap
- What is tcpdump & winpcap & snoop
- What is BPF
- What is gopacket

- [使用tc netem 模拟网络异常 | cizixs ](http://cizixs.com/2017/10/23/tc-netem-for-terrible-network)

#### 2.2 Netfilter

- [赵亚：Linux的Netfilter框架深度思考-对比Cisco的ACL](http://blog.csdn.net/dog250/article/details/6572779)
- [赵亚：探索iptables BPF模块的悲惨历程](http://blog.csdn.net/dog250/article/details/9103817)

## 三、模拟仿真工具

- [Project Quagga](https://www.quagga.net/)
- [使用 Quagga 将你的 CentOS 变成 OSPF 路由器](https://linux.cn/article-4232-1.html)

## 资源
- [Top 100 Computer Networking Blogs and Websites To Follow in 2018](https://blog.feedspot.com/networking_blogs/)

## 参考文献
- [赵亚：用Netcat，SSH构建的IP层加密隧道搭建VPN](http://blog.csdn.net/dog250/article/details/71248888)
- [赵亚： 从一个简单的聊天程序SimpleChat看VPN技术](http://blog.csdn.net/dog250/article/details/71076357)
- [赵亚：BadVPN详解之--组网原理剖析](http://blog.csdn.net/dog250/article/details/70343660)
- [赵亚：通信网络与IP网络底层传输技术梳理(SONET/SDH/OTN/ATM/Ethernet/MPLS/PTN...)](http://blog.csdn.net/dog250/article/details/69668590)
