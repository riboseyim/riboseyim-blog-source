---
title: Protocol-Route
date: 2018-11-21 16:24:02
tags:
categories: 工程技术
---
## 摘要
<!--more-->

## GRE

GRE（Generic Routing Encapsulation，通用路由封装）协议用来对某种协议（如IP、MPLS、以太网）的数据报文进行封装，使这些被封装的数据报文能够在另一个网络中传输。封装后的数据报文在网络中传输的路径，称为 GRE 隧道。GRE隧道是一个虚拟的点到点的连接，其两端的设备分别对数据报文进行封装及解封装。

GRE封装后的报文包括如下几个部分：
- 载荷包（ Payload packet ）：需要封装和传输的数据报文。净荷数据的协议类型，称为乘客协议（Passenger Protocol）。乘客协议可以是任意的网络层协议。
- GRE Header ：采用GRE协议对净荷数据进行封装所添加的报文头，包括封装层数、版本、乘客协议类型、校验和信息、Key信息等内容。添加GRE头后的报文称为GRE报文。对净荷数据进行封装的GRE协议，称为封装协议（Encapsulation Protocol）。
- 传输协议的报文头（Delivery header）：在GRE报文上添加的报文头，以便传输协议对GRE报文进行转发处理。传输协议（Delivery Protocol 或者 Transport Protocol）是指负责转发GRE报文的网络层协议。

## IGP

#### OSPF

#### IS-IS

#### 路由算法

- 最短路径树算法

## BGP

#### 路由反射器 RR

#### 路由与VPN

## NetFlow

## MPLS VPN

## 资源

- [思科网络学习空间](http://www.clnchina.com.cn/nextlevel/)

## 参考文献
- [教程|BGP技术：路由反射器|Ender.joe(周亚军)|RS CCIE,SP CCIE,思科认证讲师#34708](http://www.clnchina.com.cn/nextlevel/BGP-route-reflector.pdf)
- [H3C|三层技术-IP业务配置指导|GRE简介](http://www.h3c.com/cn/d_201901/1143179_30005_0.htm#_Toc533785111)
- [CCIE Blog|BGP Always-Compare-Med](https://ccieblog.co.uk/bgp/bgp-always-compare-med)
- [河海大学提出一种使水下网络更可靠的路由方案 | Michelle Hampson  IEEE电气电子工程师学会](https://mp.weixin.qq.com/s/529xVBPQ4q6mnbJnKa8mPg)
- [使用 RouterSploit 攻击路由器的方法介绍 | hackerbirder  看雪学院](https://mp.weixin.qq.com/s/qp3MYG__9DSw3nFPib-Nkg)
