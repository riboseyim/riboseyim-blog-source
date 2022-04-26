---
title: How Linux Works:User Space
date: 2019-04-21 12:04:15
tags: [SRE,Linux,DevOps,架构师]
categories: 工程技术
---
## 摘要

Linux 用户空间启动顺序

<!--more-->

## How User Space Starts


1. init
2. 必要的低层服务例如：udevd 和 syslog
3. 网络配置
4. 中高层服务例如 ：cron , printing
5. 登录提示、图形界面及其它高层次应用

#### 一号进程

init（initialization的简写）是 Unix 和 类Unix 系统中用来产生其它所有进程的程序。它以守护进程的方式存在，其进程号为1。Linux系统在开机时加载Linux内核后，便由Linux内核加载init程序，由init程序完成余下的开机过程，比如加载运行级别，加载服务，引导Shell/图形化界面等等。

```bash
[root@li1437-101 ~]# ps -ef | grep init
root         1     0  0 Feb27 ?        00:03:05 /sbin/init
root     28683 28663  0 02:44 pts/0    00:00:00 grep --color=auto init
```

```bash
// Mac OS
bash-3.2$ ps -ef | grep init
	0   243     1   0 15 517  ??         0:00.74 /System/Library/CoreServices/CrashReporterSupportHelper server-init
	0   533     1   0 15 517  ??         0:02.07 /System/Library/CoreServices/SubmitDiagInfo server-init
  501 52150     1   0 日01下午 ??         0:15.49 /usr/libexec/secinitd
	0 69864     1   0 11:35上午 ??         0:00.20 /usr/libexec/secinitd
	0 72830     1   0  1:51下午 ??         0:00.19 /usr/libexec/secinitd
Darwin ACA80166.ipt.aol.com 16.5.0 Darwin Kernel Version 16.5.0: Fri Mar  3 16:52:33 PST 2017; root:xnu-3789.51.2~3/RELEASE_X86_64 x86_64
bash-3.2$
```

在Linux发行版中，init有三种主要的实现形式：
1. **System V init**: 传统的
2. **systemd**: 所有主流Linux发行版中的标准init
3. **Upstart**: Ubuntu

Android 和 BSD （运行存放于'/etc/rc'的初始化 shell 脚本）也有它们自己的init版本，一些发行版也将System V init 修改为类似BSD风格的实现。目前大部分Linux发行版都已采用新的systemd替代System V和Upstart，但systemd向下兼容System V。

**System V init**: 存在一个启动序列，同一时间只能启动一个任务，这种架构下，很容易解决依赖问题，但是性能方面要受一些影响。
**systemd is goal oriented.** : 针对System V init的不足，systemd所有的服务都并发启动。systemd时基于目标的，需要定义要实现的目标，以及它的依赖项。systemd 将所有过程都抽象为一个配置单元，即 unit。可以认为一个服务是一个配置单元；一个挂载点是一个配置单元。

![systemd Unit target Tree](http://riboseyim-qiniu.riboseyim.com/Linux_kernel_systemd_UnitTree.png)

**Upstart is reactionary.**:Upstart是基于事件的，Upstart的事件驱动模型允许它以异步方式对生成的事件作出回应。

![System V init Vs UpStart Vs Systemd](http://riboseyim-qiniu.riboseyim.com/Linux_kernel_systemd_upstart_sysV.png)

## The Initial RAM filesystem

Linux内核不能通过访问PC BIOS 或者 EFI接口从磁盘获取数据，所以为了mount它的root filesystem, 对于底层存储需要驱动程序支持。解决方案是在内核运行之前，由boot loader加载驱动模块及工具到内存。在启动时，内核读取相关模块到一个临时的RAM filesystem(initramfs),挂载在／根目录,initramsfs允许内核为真正的root filesystem加载必要的驱动模块。
最后，再挂载真正的root filesystem、启动init。

Linux在很多场景下都需要创建一个基于内存的文件系统，提供一个可以接近零延迟的快速存储区域。目前有两类主要的RAM磁盘可用，她们个有优劣：ramfs和tmpfs。(注意：创建之前可以使用 **free** 命令查看未使用的RAM)

```C
# mkdir /mnt/ramdisk
# mount -t tmpfs -o size=512m tmpfs /mnt/ramdisk
# vi /etc/fstab
#tmpfs       /mnt/ramdisk tmpfs   nodev,nosuid,noexec,nodiratime,size=1024M   0 0
```

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
