---
title: How Linux Works:Boots
date: 2017-05-29 20:39:19
tags: [SRE,Linux,DevOps,架构师]
categories: 工程技术
---
## 摘要
- 一、How the Linux Kernel Boots
- 二、How User Space Starts
- 三、The Initial RAM filesystem

<!--more-->

>史蒂夫·乔布斯（Steve Jobs）：“假设你可以缩短10秒钟的开机时间，把这个乘上500万，那就是每天5000万秒了。一年下来大概是好几十辈子的时间。想想看，如果你可以让开机速度快10秒钟的话，就拯救了数十条生命。这很值得啊，你不觉得吗？”  《硅谷革命：成就苹果公司的疯狂往事》

## How the Linux Kernel Boots
1. The machine’s BIOS or boot firmware loads and runs a boot loader.(Boot Loader 是在操作系统内核运行之前运行的一段小程序，它严重地依赖于硬件而实现)
2. The boot loader finds the kernel image on disk, loads it into memory, and starts it. （选择内核镜像，加载到内存空间，为最终调用操作系统内核准备好正确的环境。）
3.  The kernel initializes the devices and its drivers.（初始化硬件设备及其驱动程序）
4.  The kernel mounts the root filesystem.（挂载根目录。根目录指文件系统的最上一级目录，它是相对子目录来说的；它如同一棵大树的“根”一般，所有的树杈以它为起点）
5.  The kernel starts a program called init with a process ID of 1. This point is the user space start.（内核启动一个初始化程序，从这里开始虚拟内存开始划分出使用者空间，与内核空间（Kernel space）对应）
6.  init sets the rest of the system processes in motion
7.  At some point, init starts a process allowing you to log in, usually at the end or near the end of the boot.

#### Startup Messages
有两种方式可以查看内核引导和运行诊断信息：
1. 查看内核系统日志文件。文件路径： /var/log/kern.log
2.  执行dmesg命令
```bash
[root@li1437-101 ~]# dmesg
[    0.000000] Linux version 4.9.7-x86_64-linode80 (maker@build) (gcc version 4.7.2 (Debian 4.7.2-5) ) #2 SMP Thu Feb 2 15:43:55 EST 2017
[    0.000000] Command line: root=/dev/sda console=tty1 console=ttyS0 ro  devtmpfs.mount=1
[    0.000000] x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
[    0.000000] x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]:  256
[    0.000000] x86/fpu: Enabled xstate features 0x7, context size is 832 bytes, using 'standard' format.
[    0.000000] x86/fpu: Using 'eager' FPU context switches.
[    0.000000] e820: BIOS-provided physical RAM map:
…….

[    0.000000] NX (Execute Disable) protection: active
[    0.000000] SMBIOS 2.8 present.
[    0.000000] DMI: QEMU Standard PC (i440FX + PIIX, 1996), BIOS rel-1.9.1-0-gb3ef39f-prebuilt.qemu-project.org 04/01/2014
[    0.000000] Hypervisor detected: KVM

……
[    0.371925] raid6: sse2x1   gen()  7490 MB/s
[    0.428689] raid6: sse2x1   xor()  5953 MB/s
[    0.485463] raid6: sse2x2   gen()  9289 MB/s
[    0.542230] raid6: sse2x2   xor()  6754 MB/s
[    0.599013] raid6: sse2x4   gen() 10954 MB/s
[    0.656189] raid6: sse2x4   xor()  5522 MB/s
[    0.656943] raid6: using algorithm sse2x4 gen() 10954 MB/s
[    0.657588] raid6: .... xor() 5522 MB/s, rmw enabled
……
[    1.053697] Netfilter messages via NETLINK v0.30.
[    1.054471] nfnl_acct: registering with nfnetlink.
[    1.055332] nf_conntrack version 0.5.0 (8192 buckets, 32768 max)
[    1.056324] ctnetlink v0.93: registering with nfnetlink.
[    1.057335] nf_tables: (c) 2007-2009 Patrick McHardy <kaber@trash.net>
[    1.058393] nf_tables_compat: (c) 2012 Pablo Neira Ayuso <pablo@netfilter.org>
[    1.059599] xt_time: kernel timezone is -0000
[    1.060296] ip_set: protocol 6
[    1.060791] IPVS: Registered protocols (TCP, UDP, SCTP, AH, ESP)
[    1.061940] IPVS: Connection hash table configured (size=4096, memory=64Kbytes)
[    1.063162] IPVS: Creating netns size=2104 id=0
[    1.064139] IPVS: ipvs loaded.
……
[    1.744221] systemd[1]: Detected virtualization kvm.
[    1.745058] systemd[1]: Detected architecture x86-64.
[    1.747402] systemd[1]: Set hostname to <localhost.localdomain>.
[    1.834328] tsc: Refined TSC clocksource calibration: 2800.119 MHz
[    1.835512] clocksource: tsc: mask: 0xffffffffffffffff max_cycles: 0x285cb16f950, max_idle_ns: 440795333193 ns
[    1.843476] systemd[1]: Created slice Root Slice.
[    1.844251] systemd[1]: Starting Root Slice.
[    1.845835] systemd[1]: Created slice System Slice.
[    1.846631] systemd[1]: Starting System Slice.
[    1.848257] systemd[1]: Listening on udev Kernel Socket.
[    1.849119] systemd[1]: Starting udev Kernel Socket.
[    2.014715] EXT4-fs (sda): re-mounted. Opts: (null)
[    2.038202] systemd-journald[2010]: Received request to flush runtime journal from PID 1
[    2.241341] audit: type=1305 audit(1488188850.897:2): audit_pid=2215 old=0 auid=4294967295 ses=4294967295 res=1
[    2.287758] Adding 262140k swap on /dev/sdb.  Priority:-1 extents:1 across:262140k FS
[    2.905177] IPVS: Creating netns size=2104 id=1
[    2.954613] IPv6: ADDRCONF(NETDEV_UP): eth0: link is not ready
[    2.955987] 8021q: adding VLAN 0 to HW filter on device eth0
[    8.009765] random: crng init done
```

在故障排查中，dmesg信息需要首先查看,例如输出最近10条系统信息,
可以查看到引起性能问题的错误。
```bash
$ dmesg | tail
[1880957.563150] perl invoked oom-killer: gfp_mask=0x280da, order=0, oom_score_adj=0
[...]
[1880957.563400] Out of memory: Kill process 18694 (perl) score 246 or sacrifice child
[1880957.563408] Killed process 18694 (perl) total-vm:1972392kB, anon-rss:1953348kB, file-r
ss:0kB
[2320864.954447] TCP: Possible SYN flooding on port 7001. Dropping request. Check SNMP cou
nters.
```

#### Kernel initialization and Boot Options

在启动时，Linux内核初始化的顺序如下：

1. CPU inspection （检查CPU）
2.  Memory inspection （检查内存）
3.  Device bus discovery （发现设备总线）
4.  Device discovery （发现设备）
5.  Auxiliary kernel subsystem setup(networking, and so on) （辅助内核子系统启动，例如网络等）
6.  Root filesystem mount （挂载根目录）
7.  User space start （用户空间启动）

![root filesystem](http://riboseyim-qiniu.riboseyim.com/Linux_kernel_root_filesystem.png)

### Kernel Parameters

文件/proc/cmdline记录了系统内核启动参数：
```bash
[root@li1437-101 ~]# cat /proc/cmdline
root=/dev/sda console=tty1 console=ttyS0 ro  devtmpfs.mount=1
```
查看运行级别：
```bash
[root@li1437-101 ~]# who -r
		 run-level 3  2017-02-27 09:47
[root@li1437-101 ~]#
```

#### 电子书《Linux Perf Master》

- https://riboseyim.gitbook.io/perf
- https://www.gitbook.com/book/riboseyim/linux-perf-master/details

![](http://riboseyim-qiniu.riboseyim.com/banner-LPM-201803.png)

#### 扩展阅读：性能诊断指南
- [Linux 性能诊断：负载评估](https://riboseyim.com/2017/12/11/Linux-Perf-Load/)
- [Linux 性能诊断：快速检查单](https://riboseyim.com/2017/12/11/Linux-Perf-Netflix/)
- [Linux 性能诊断：JVM](https://riboseyim.com/2018/08/07/Linux-Perf-JVM/)

#### 扩展阅读：How Linux Works
- [How Linux Works：The Big Picture](https://riboseyim.com/2019/04/21/Linux-Works/)
- [How Linux Works：BASIC Commands](https://riboseyim.com/2017/04/26/Linux-Commands/)
- [How Linux Works：BASIC Commands Extension](https://riboseyim.com/2018/09/03/Linux-Commands-New/)
- [How Linux Works：Device and FileSystem](https://riboseyim.com/2018/06/07/Linux-Works-FileSystem/)
- [How Linux Works：Boots](https://riboseyim.com/2017/05/29/Linux-Works-Boots/)
- [How Linux Works：用户空间](https://riboseyim.com/2019/04/21/Linux-Works-User-Space/)
- [How Linux Works：内存管理](https://riboseyim.com/2017/12/11/Linux-Works-Memory/)
- [How Linux Works：网络管理](https://riboseyim.com/2018/01/08/Linux-Works-Network/)
- Preview[How Linux Works：路由管理](https://riboseyim.com/2019/03/05/Linux-Works-Router/)

#### 扩展阅读：动态追踪技术
- [动态追踪技术(一)：DTrace 导论](https://riboseyim.com/2016/11/26/DTrace/)
- [动态追踪技术(二)：strace+gdb 溯源 Nginx 内存溢出异常 ](https://mp.weixin.qq.com/s?__biz=MjM5MTY1MjQ3Nw==&mid=2651939588&idx=1&sn=35f71c5f88d1edf23cb2efc812ab8e6c&chksm=bd578c168a20050041c08618281691f0111f61c789097a69095933057618637fc54817815921#rd)
- [动态追踪技术(三)：Tracing Your Kernel Function!](https://riboseyim.com/2017/04/17/DTrace_FTrace/)
- [动态追踪技术(四)：基于 Linux bcc/BPF 实现 Go 程序动态追踪](https://riboseyim.com/2017/06/27/DTrace_bcc/)
- [动态追踪技术(五)：Welcome DTrace for Linux](https://riboseyim.com/2018/02/16/DTrace-Linux/)

## 参考文献
- 《Linux 的启动过程》，白崎博生，2004 （暂无中文版）
- 《深入理解 Linux 内核》，Daniel P.Bovert （经典）
- [Inside the Linux boot process](https://www.ibm.com/developerworks/library/l-linuxboot/)
- [linfo.org:Root Filesystem Definition](http://www.linfo.org/root_filesystem.html)
- [阮一峰：Systemd 入门教程：命令篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)
- [阮一峰：Systemd 入门教程：实战篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)
- [IBM developerworks:浅析 Linux 初始化 init 系统，第 3 部分: Systemd](https://www.ibm.com/developerworks/cn/linux/1407_liuming_init3/)
- [维基百科：Systemd](https://zh.wikipedia.org/wiki/Systemd)
- [differences between ramfs and tmpfs](http://www.jamescoyle.net/knowladge/951-the-difference-between-a-tmpfs-and-ramfs-ram-disk)
- [Unix Background (Signal、Messages & Queue) | Simon Hørup Eskildsen](http://sirupsen.com/unix-background-queue/)
- [(推荐) How the Kernel Manages Your Memory | Gustavo Duarte ](http://duartes.org/gustavo/blog/post/how-the-kernel-manages-your-memory/)
- [(推荐) What Your Computer Does While You Wait | Gustavo Duarte ](http://duartes.org/gustavo/blog/post/what-your-computer-does-while-you-wait/)
- [(推荐) What Does an Idle CPU Do? | Gustavo Duarte ](http://duartes.org/gustavo/blog/post/what-does-an-idle-cpu-do/)
