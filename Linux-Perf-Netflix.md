---
title: Linux 性能诊断:快速检查单(Netflix版)
date: 2017-12-11 10:01:10
tags: [SRE,DevOps,Linux]
categories: 工程技术
---
## 摘要
- 快速检查单QRH

>请大家记住这样一个思路：先把大石头移开。大石头移开后，中等大小的石头可能就显现出来了。性能调优的原则就是先解决大问题，然后解决剩余问题中的大问题。在解决了大问题后，通常隐藏在它背后的问题也会暴露出来。也就是说，解决了前面的瓶颈后，下一个瓶颈就显现出来了。—— 《图解性能优化》

<!--more-->

## 快速检查单

快速检查单（Quick Reference Handbook，QRH）是飞行员在飞行过程中依赖的重要指导性文件。

第一张飞行检查单起源于一次严重的航空事故。1935年波音公司研制的一架新型轰炸机在试飞过程中突然坠机，导致2名机组人员遇难——包括一名最优秀的试飞员普洛耶尔·希尔少校。后来的调查结果分析，事故并不是机械故障引起的，而是人为失误造成。新型飞机比以往的飞机更复杂，飞行员要管理4台发动机，操控起落架、襟翼、电动配平调整片和恒速液压变距螺旋桨等。因为忙于各种操作，希尔少校忘记了一项简单却很重要的工作 —— 在起飞前忘记对新设计的升降舵和方向舵实施解锁。

美国军方组织飞行专家编制了一份飞行检查单，将起飞、巡航、着陆和滑行各阶段的重要步骤写在一张索引卡片上。飞行员根据检查单的提示检查刹车是否松开，飞行仪表是否准确设定，机舱门窗是否完全关闭，升降舵等控制面是否已经解锁。

## Netflix 性能工程团队

登陆一台 Linux 服务器排查性能问题：**开始一分钟你该检查哪些呢？**
在 Netflix 我们有一个庞大的 EC2 Linux集群，也有许多性能分析工具用于监视和检查它们的性能。它们包括用于云监测的Atlas (工具代号) ，用于实例分析的 Vector (工具代号) 。尽管这些工具能帮助我们解决大部分问题，我们有时也需要登陆一台实例、运行一些标准的 Linux 性能分析工具。在这篇文章，Netflix 性能工程团队将向您展示：在开始的60秒钟，利用标准的Linux命令行工具，执行一次充分的性能检查。

## Linux 性能分析黄金60秒

运行以下10个命令，你可以在60秒内，获得系统资源利用率和进程运行情况的整体概念。查看是否存在异常、评估饱和度，它们都非常易于理解，可用性强。饱和度表示资源还有多少负荷可以让它处理，并且能够展示请求队列的长度或等待的时间。

```
uptime
dmesg | tail vmstat 1
mpstat -P ALL 1 pidstat 1
iostat -xz 1 free -m
sar -n DEV 1
sar -n TCP,ETCP 1 top
```

- ![性能检查的一般步骤](http://upload-images.jianshu.io/upload_images/1037849-9981ca123c9bc27a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这些命令需要安装sysstat包。这些命令输出的指标，将帮助你掌握一些有效的方法：一整套寻找性能瓶颈的方法论。这些命令需要检查所有资源的利用率、饱和度和错误信息（CPU、内存、磁盘等）。同时，当你检查或排除一些资源的时候，需要注意在检查过程中，根据指标数据指引，逐步缩小目标范围。

接下来的章节，将结合生产环境的案例演示这些命令。如果希望了解这些工具的详细信息，可以查阅它们的操作文档。

#### 1. uptime

```
$ uptime
23:51:26up21:31, 1user, loadaverage:30.02,26.43,19.02
```

这是一个快速查看平均负载的方法，表示等待运行的任务（进程）数量。在Linux系统中，这些数字包含等待CPU运行的进程数，也包括不间断I/O阻塞的进程数（通常是磁盘I/O）。它展示了一个资源负载（或需求）的整体概念，但是无法理解其中的内涵，在没有其它工具的情况下。仅仅是一种快速查看手段而已。

这三个数字呈现出平均负载在几何级减弱，依次表示持续1分钟，5分钟和15分钟内。这三个数字能告诉我们负载在时间线上是如何变化的。举例说明，如果你在一个问题服务器上执行检查，1分钟的值远远低于15分钟的值，可以判断出你也许登录得太晚了，已经错过了问题。

在上面的例子中，平均负载的数值显示最近正在上升，1分钟值高达30，对比15分钟值则是19。这些指标值像现在这么大意味着很多情况：也许是CPU繁忙；vmstat 或者 mpstat 将可以确认，本系列的第三和第四条命令。

#### 2. dmesg | tail

```C
$ dmesg | tail
[1880957.563150] perl invoked oom-killer: gfp_mask=0x280da, order=0, oom_score_adj=0
[...]
[1880957.563400] Out of memory: Kill process 18694 (perl) score 246 or sacrifice child
[1880957.563408] Killed process 18694 (perl) total-vm:1972392kB, anon-rss:1953348kB, file-r
ss:0kB
[2320864.954447] TCP: Possible SYN flooding on port 7001. Dropping request. Check SNMP cou
nters.
```

这个结果输出了最近10条系统信息。可以查看到引起性能问题的错误。上面的例子包含了oom-killer,以及TCP丢包。

>译者注:除了error级的日志，info级的也要留个心眼，可能包含一些隐藏信息。

#### 3. vmstat 1

```C
$ vmstat 1
procs ---------memory---------- ---swap-- -----io---- -system-- ------cpu-----
r b swpd free buff cache si so bi bo in cs us sy id wa st
34 0 0 200889792 73708 591828 0 0 0 5 6 10 96 1 3 0 0
32 0 0 200889920 73708 591860 0 0 0 592 13284 4282 98 1 1 0 0
32 0 0 200890112 73708 591860 0 0 0 0 9501 2154 99 1 0 0 0
32 0 0 200889568 73712 591856 0 0 0 48 11900 2459 99 0 0 0 0
32 0 0 200890208 73712 591860 0 0 0 0 15898 4840 98 1 1 0 0
```
**vmstat** 是一个获得虚拟内存状态概况的通用工具（最早创建于10年前的BSD）。它每一行记录了关键的服务器统计信息。vmstat 运行的时候有一个参数1，用于输出一秒钟的概要数据。第一行输出显示启动之后的平均值，用以替代之前的一秒钟数据。现在，跳过第一行，让我们来学习并且记住每一列代表的意义。

**r**：正在CPU上运行或等待运行的进程数。相对于平均负载来说，这提供了一个更好的、用于查明CPU饱和度的指标，它不包括I/O负载。注: “r”值大于CPU数即是饱和。

**free**: 空闲内存（kb)
如果这个数值很大，表明你还有足够的内存空闲。包括命令7“free m”，很好地展现了空闲内存的状态。

**si, so**: swap入／出。如果这个值非0，证明内存溢出了。

**us, sy, id, wa, st**:它们是CPU分类时间，针对所有CPU的平均访问。分别是用户时间，系统时间（内核），空闲，I/O等待时间，以及被偷走的时间（其它访客，或者是Xen）。CPU分类时间将可以帮助确认，CPU是否繁忙，通过累计用户系统时间。等待I/O的情形肯定指向的是磁盘瓶颈；这个时候CPU通常是空闲的，因为任务被阻塞以等待分配磁盘I/O。你可以将等待I/O当作另一种CPU空闲，一种它们为什么空闲的解释线索。

系统时间对I/O处理非常必要。一个很高的平均系统时间，超过20%，值得深入分析：也许是内核处理I/O非常低效。在上面的例子中，CPU时间几乎完全是用户级的，与应用程序级的利用率正好相反。所有CPU的平均利用率也超过90%。这不一定是一个问题；还需检查“r”列的饱和度。

#### 4. mpstat P ALL 1

```C
$ mpstat -P ALL 1
Linux 3.13.0-49-generic (titanclusters-xxxxx) 07/14/2015 _x86_64_ (32 CPU)
07:38:49 PM CPU %usr %nice %sys %iowait %irq %soft %steal %guest %gnice %idle
07:38:50 PM all 98.47 0.00 0.75 0.00 0.00 0.00 0.00 0.00 0.00 0.78
07:38:50 PM 0 96.04 0.00 2.97 0.00 0.00 0.00 0.00 0.00 0.00 0.99
07:38:50 PM 1 97.00 0.00 1.00 0.00 0.00 0.00 0.00 0.00 0.00 2.00
07:38:50 PM 2 98.00 0.00 1.00 0.00 0.00 0.00 0.00 0.00 0.00 1.00
07:38:50 PM 3 96.97 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 3.03
[...]
```
这个命令可以按时间线打印每个CPU的消耗，常常用于检查不均衡的问题。如果只有一个繁忙的CPU，可以判断是属于单进程的应用程序。

#### 5. pidstat 1

```C
$ pidstat 1
Linux 3.13.0-49-generic (titanclusters-xxxxx) 07/14/2015 _x86_64_ (32 CPU)
07:41:02 PM UID PID %usr %system %guest %CPU CPU Command
07:41:03 PM 0 9 0.00 0.94 0.00 0.94 1 rcuos/0
07:41:03 PM 0 4214 5.66 5.66 0.00 11.32 15 mesos-slave
07:41:03 PM 0 4354 0.94 0.94 0.00 1.89 8 java
07:41:03 PM 0 6521 1596.23 1.89 0.00 1598.11 27 java
07:41:03 PM 0 6564 1571.70 7.55 0.00 1579.25 28 java
07:41:03 PM 60004 60154 0.94 4.72 0.00 5.66 9 pidstat
07:41:03 PM UID PID %usr %system %guest %CPU CPU Command
07:41:04 PM 0 4214 6.00 2.00 0.00 8.00 15 mesos-slave
07:41:04 PM 0 6521 1590.00 1.00 0.00 1591.00 27 java
07:41:04 PM 0 6564 1573.00 10.00 0.00 1583.00 28 java
07:41:04 PM 108 6718 1.00 0.00 0.00 1.00 0 snmp-pass
07:41:04 PM 60004 60154 1.00 4.00 0.00 5.00 9 pidstat
^C
```
pidstat 有一点像顶级视图－针对每一个进程，但是输出的时候滚屏，而不是清屏。它非常有用，特别是跨时间段查看的模式，也能将你所看到的信息记录下来，以利于进一步的研究。上面的例子识别出两个 java 进程引起的CPU耗尽。“％CPU” 是对所有CPU的消耗；1591% 显示 java 进程占用了几乎16个CPU。

#### 6. iostat xz 1

```C
$ iostat -xz 1
Linux 3.13.0-49-generic (titanclusters-xxxxx) 07/14/2015 _x86_64_ (32 CPU)
avg-cpu: %user %nice %system %iowait %steal %idle
73.96 0.00 3.73 0.03 0.06 22.21
Device: rrqm/s wrqm/s r/s w/s rkB/s wkB/s avgrq-sz avgqu-sz await r_await w_await svctm %util
xvda 0.00 0.23 0.21 0.18 4.52 2.08 34.37 0.00 9.98 13.80 5.42 2.44 0.09
xvdb 0.01 0.00 1.02 8.94 127.97 598.53 145.79 0.00 0.43 1.78 0.28 0.25 0.25
xvdc 0.01 0.00 1.02 8.86 127.79 595.94 146.50 0.00 0.45 1.82 0.30 0.27 0.26
dm-0 0.00 0.00 0.69 2.32 10.47 31.69 28.01 0.01 3.23 0.71 3.98 0.13 0.04
dm-1 0.00 0.00 0.00 0.94 0.01 3.78 8.00 0.33 345.84 0.04 346.81 0.01 0.00
dm-2 0.00 0.00 0.09 0.07 1.35 0.36 22.50 0.00 2.55 0.23 5.62 1.78 0.03
[...]

```
这是一个理解块设备（磁盘）极好的工具，不论是负载评估还是作为性能测试成绩。

**r/s, w/s, rkB/s, wkB/s**: 这些是该设备每秒读％、写％、读Kb、写Kb。可用于描述工作负荷。一个性能问题可能只是简单地由于一个过量的负载引起。

**await**: I/O平均时间（毫秒）
这是应用程序需要的时间，它包括排队以及运行的时间。
远远大于预期的平均时间可以作为设备饱和，或者设备问题的指标。

**avgqu­sz**: 向设备发出的平均请求数。
值大于1可视为饱和（尽管设备能对请求持续运行，特别是前端的虚拟设备－后端有多个磁盘）。

**%util**: 设备利用率
这是一个实时的繁忙的百分比，显示设备每秒钟正在进行的工作。值大于60%属于典型的性能不足（可以从await处查看），尽管它取决于设备。值接近100% 通常指示饱和。￼￼如果存储设备是一个前端逻辑磁盘、后挂一堆磁盘，那么100%的利用率也许意味着，一些已经处理的I/O此时占用100%，然而，后端的磁盘也许远远没有达到饱和，其实可以承担更多的工作。

切记：磁盘I/O性能低并不一定是应用程序问题。许多技术一贯使用异步I/O，所以应用程序并不会阻塞，以及遭受直接的延迟（例如提前加载，缓冲写入）。

#### 7. free m

```C
$ free -m
total used free shared buffers cached
Mem: 245998 24545 221453 83 59 541
-/+ buffers/cache: 23944 222053
Swap: 0 0 0
```
**buffers**: buffer cache,用于块设备I/O。
**cached**:page cache, 用于文件系统。
￼ ￼
我们只是想检查这些指标值不为0——那样意味着磁盘I/O高、性能差（确认需要用iostat）。上面的例子看起来不错，每一类内存都有富余。

**“­/+ buffers/cache”**: 提供了关于内存利用率更加准确的数值。

Linux可以将空闲内存用于缓存，并且在应用程序需要的时候收回。所以应用到缓存的内存必须以另一种方式包括在内存空闲的数据里面。有一个网站[linux ate my ram](http://www.linuxatemyram.com/),专门探讨这个困惑。它还有更令人困惑的地方，如果在Linux上使用ZFS,正如我们运行一些服务，ZFS拥有自己的文件系统缓存，也不能在free -m 的输出里正确反映。这种情况会显示系统空闲内存不足，但是内存实际上可用，通过回收 ZFS 的缓存。

关于 Linux 内存管理的更多内容，可以阅读[操作系统原理：How Linux Works (Memroy)](https://riboseyim.github.io/2017/12/11/Linux-Works-Memory/)。

#### 8. sar n DEV 1

```C
$ sar -n DEV 1
Linux 3.13.0-49-generic (titanclusters-xxxxx) 07/14/2015 _x86_64_ (32 CPU)
12:16:48 AM IFACE rxpck/s txpck/s rxkB/s txkB/s rxcmp/s txcmp/s rxmcst/s %ifutil
12:16:49 AM eth0 18763.00 5032.00 20686.42 478.30 0.00 0.00 0.00 0.00
12:16:49 AM lo 14.00 14.00 1.36 1.36 0.00 0.00 0.00 0.00
12:16:49 AM docker0 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
12:16:49 AM IFACE rxpck/s txpck/s rxkB/s txkB/s rxcmp/s txcmp/s rxmcst/s %ifutil
12:16:50 AM eth0 19763.00 5101.00 21999.10 482.56 0.00 0.00 0.00 0.00
12:16:50 AM lo 20.00 20.00 3.25 3.25 0.00 0.00 0.00 0.00
12:16:50 AM docker0 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
^C
```

使用这个工具用来检查网络接口吞吐量：**rxkB/s** 和** txkB/s**, 作为负载的一种度量方式, 也可以用来检查是否已经达到某种瓶颈。

在上面的例子中，网卡 eth0 收包大道 22 Mbytes/s, 即176 Mbits/sec (就是说，在 1 Gbit/sec 的限制之内)。此版本也有一个体现设备利用率的 “％ifutil” （两个方向最大值），我们也可以使用 Brendan的nicstat 工具来度量。和 nicstat 类似，这个值很难准确获取，看起来在这个例子中并没有起作用（0.00）。

#### 9. sar n TCP,ETCP 1

```C
$ sar -n TCP,ETCP 1
Linux 3.13.0-49-generic (titanclusters-xxxxx) 07/14/2015 _x86_64_ (32 CPU)
12:17:19 AM active/s passive/s iseg/s oseg/s
12:17:20 AM 1.00 0.00 10233.00 18846.00
12:17:19 AM atmptf/s estres/s retrans/s isegerr/s orsts/s
12:17:20 AM 0.00 0.00 0.00 0.00 0.00
12:17:20 AM active/s passive/s iseg/s oseg/s
12:17:21 AM 1.00 0.00 8359.00 6039.00
12:17:20 AM atmptf/s estres/s retrans/s isegerr/s orsts/s
12:17:21 AM 0.00 0.00 0.00 0.00 0.00
^C
```

这是一个关键TCP指标的概览视图。包括：
- **active/s**: 本地初始化的 TCP 连接数 ／每秒（例如，通过connect() ）
- **passive/s**: 远程初始化的 TCP 连接数／每秒（例如，通过accept() ）
- **retrans/s**: TCP重发数／每秒

这些活跃和被动的计数器常常作为一种粗略的服务负载度量方式：新收到的连接数 (被动的),以及下行流量的连接数 (活跃的)。这也许能帮助我们理解，活跃的都是外向的，被动的都是内向的，但是严格来说这种说法是不准确的（例如，考虑到“本地－本地”的连接）。重发数是网络或服务器问题的一个标志；它也许是因为不可靠的网络（如，公共互联网），也许是由于一台服务器已经超负荷、发生丢包。
上面的例子显示每秒钟仅有一个新的TCP连接。

#### 10. top

```C
$ top
top - 00:15:40 up 21:56, 1 user, load average: 31.09, 29.87, 29.92
Tasks: 871 total, 1 running, 868 sleeping, 0 stopped, 2 zombie
%Cpu(s): 96.8 us, 0.4 sy, 0.0 ni, 2.7 id, 0.1 wa, 0.0 hi, 0.0 si, 0.0 st
KiB Mem: 25190241+total, 24921688 used, 22698073+free, 60448 buffers
KiB Swap: 0 total, 0 used, 0 free. 554208 cached Mem
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
20248 root 20 0 0.227t 0.012t 18748 S 3090 5.2 29812:58 java
4213 root 20 0 2722544 64640 44232 S 23.5 0.0 233:35.37 mesos-slave
66128 titancl+ 20 0 24344 2332 1172 R 1.0 0.0 0:00.07 top
5235 root 20 0 38.227g 547004 49996 S 0.7 0.2 2:02.74 java
4299 root 20 0 20.015g 2.682g 16836 S 0.3 1.1 33:14.42 java
1 root 20 0 33620 2920 1496 S 0.0 0.0 0:03.82 init
2 root 20 0 0 0 0 S 0.0 0.0 0:00.02 kthreadd
3 root 20 0 0 0 0 S 0.0 0.0 0:05.35 ksoftirqd/0
5 root 0 -20 0 0 0 S 0.0 0.0 0:00.00 kworker/0:0H
6 root 20 0 0 0 0 S 0.0 0.0 0:06.94 kworker/u256:0
8 root 20 0 0 0 0 S 0.0 0.0 2:38.05 rcu_sched
```

top命令包含了许多我们之前已经检查的指标。它可以非常方便地运行，看看是否任何东西看起来与从前面的命令的结果完全不同，可以表明负载指标是不断变化的。顶部下面的输出，很难按照时间推移的模式查看，可能使用如 vmstat 和 pidstat 等工具会更清晰，它们提供滚动输出。如果你保持输出的动作不够快 （CtrlS 要暂停，CtrlQ 继续），屏幕将清除，间歇性问题的证据也会丢失。

## 总结
故障检查过程中，人的作用主要是作出决策。遗忘、遗漏、麻痹、松懈是每个人都会犯的错误，好的公司都会根据经验编制检查单，提高工作效率，降低人为失误发生的概率。出于竞争因素考虑，应该充分重视检查单的更新、完善、自动化，以此为基础建立自己的技术壁垒。

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
- [Netflix Technology Blog:Linux Performance Analysis in 60,000 Milliseconds](https://medium.com/netflix-techblog/linux-performance-analysis-in-60-000-milliseconds-accc10403c55)
- [从《清单革命》看飞行检查单](http://news.carnoc.com/list/409/409763.html)
