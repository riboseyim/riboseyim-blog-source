---
title: How Linux Works
date: 2019-04-21 11:49:13
tags: [SRE,Linux,DevOps,架构师]
categories: 工程技术
---
## 摘要

- 第一部分 The Big Picture
- Levels and Layers
- Hardware
- The Kernel
- User and User Space
- 第二部分 Overview of the Linux Kernel
- 进程管理 Process Management
- 内存管理 Memory Management
- 设备管理 Device Drivers and Management
- 系统调用 System Calls
- 用户空间 User Space
- 用户管理 User Management
- 第三部分 Application and Development
- The Desktop
- The Shell Script
- The Development Tools
- Compiling and Building
- 第四部分 Future

<!--more-->

# 第一部分 The Big Picture

### Levels and Layers

### Hardware

### Overview of the Linux Kernel
#### Basic Commands
#### 设备管理 Device Drivers and Management
#### 磁盘管理 DISKS and Filesystems
#### 进程管理 Process Management
#### 内存管理 Memory Management
#### 系统调用 System Calls
#### 用户空间 User Space
#### 用户管理 User Management

### 未来 Looking Forward

# 第二部分 Overview of the Linux Kernel

## 一、Basic Commands

- [How Linux Works：BASIC Commands](https://riboseyim.com/2017/04/26/Linux-Commands/)
- [How Linux Works：BASIC Commands Extension](https://riboseyim.com/2018/09/03/Linux-Commands-New/)

## 二、Files and Directory

#### Files Modes and Permissions

- [Chmod Command Examples](https://linuxhandbook.com/chmod-command/?utm_campaign=chmod-command-examples&utm_medium=social_link&utm_source=missinglettr)

#### Listing and Manipulating Processes

#### Directory Hierarchy

#### Running Commands

## 三、DEVICES 设备管理

### Devices Files

### Devices Path

### Devices Extend

## 四、DISKS and FileSystems 磁盘管理

- [How Linux Works：Device and FileSystem](https://riboseyim.com/2018/06/07/Linux-Works-FileSystem/)

### Partitioning Disk Devices

### FileSystems

### Swap Space

### Traditional FileSystems

### Looking Forwarding

## 五、How the Linux Kernel Boots

- [How Linux Works：Boots](https://riboseyim.com/2017/05/29/Linux-Works-Boots/)

## 六、How User Space Starts

- [How Linux Works：用户空间](https://riboseyim.com/2019/04/21/Linux-Works-User-Space/)

## 七、System Configuration

### System Logging

### User Management Files

- /etc/passwd File
- /etc/shadow File

### System Time

### Batch Jobs

#### Scheduling Tasks

### Users

#### User ID and Users Switching

#### User Identification and Authentication

## 八、System Resource Utilization

### Introduction to Resource Monitoring

### Load Averages

- [Linux 性能诊断：负载评估](https://riboseyim.com/2017/12/11/Linux-Perf-Load/)
- [Linux 性能诊断：快速检查单](https://riboseyim.com/2017/12/11/Linux-Perf-Netflix/)

### Measuring Memory

### Measuring CPU

### Measuring I/O

### Per-Processes Monitoring

## 九、Network

### Network Basic

### Moving Files across the Network

#### rsync

#### samba

#### NFS

### Configuring Linux as a Router

### Firewalls

### Ethernet,IP and ARP

### Wireless Ethernet

## 十、Network Application and Services

### Network Servers

### SSH

### Diagnostic Tools

- lsof
- tcpdump
- netstat
- port scanning

### Remote Procedure Call(RPC)

### Network Security

# 第三部分 Application and Development

## 一、The Desktop

## 二、Shell Scripts

- [How Linux Works：Shell](https://riboseyim.github.io/2019/05/12/Linux-Shell/)

## 三、Development Tools

## 四、Compiling and Building

### Compiling Software From C Source Code

### Building on the BASIC

- Web Servers and Application
- Databasees
- Virtualization
- Distributed Computing
- Embedded Systems

# 第四部分 Future

## Tips

#### 源码阅读的一般方法

- 核心子系统（例如进程管理子系统）
- 结构体、数据结构
- 关键程序、加载顺序
- 主题式探索（例如：Linux 支持闰秒吗？）

## 扩展阅读

#### 电子书《Linux Perf Master》

- https://riboseyim.gitbook.io/perf
- https://www.gitbook.com/book/riboseyim/linux-perf-master/details

![](http://riboseyim-qiniu.riboseyim.com/banner-LPM-201803.png)

#### 性能诊断指南
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

## 参考文献
- [How to check List users in Linux | Complete Guide for Beginners](https://www.cyberpratibha.com/list-users-linux/)
- [How To Configure Authenticated NTP Using Symmetric Keys (compatibility with FIPS 140-2)](https://access.redhat.com/solutions/393663)
