---
title: 专题：动态追踪技术
date: 2016-11-26 01:21:31
tags: [DTrace,Linux,DevOps]
categories: 工程技术
---
## 摘要
- 动态追踪技术：DTrace for Linux
- Linux 追踪器选型

>应用软件运行速度提升的关键在于有一个好的性能分析器(profiler)帮助指导程序开发。《黑客与画家》（Hackers & Painters）

<!--more-->

## Introduction to DTrace

#### What is DTrace

通过前面几篇文章的介绍，我们已经可以通过一系列命令，从不同维度获得操作系统当前的性能运行情况。另外，借助类似 [Ganglia](https://riboseyim.github.io/2016/11/04/OpenSource-Ganglia/) 这样的开源产品，持续不断地实施性能数据采集和存储，我们基于时间序列的历史性能图形，就可以大致判读出计算集群的资源消耗情况和变化趋势。但是，仅仅这些还是不够的，在很多情况下，我们希望能够知道：“慢，是为什么慢；快，又是为什么快”。如果要回答这个问题，就必须引入另外一件神兵利器：**动态追踪技术（Dynamic Tracing）**。

鉴于这套兵器过于复杂（牛逼），属于专家级技能， advanced performance analysis and troubleshooting tool。据称掌握该技能需要耗费大约100小时以上，所以如果不是对于系统性能问题有极致追求，以及变态般地技术狂热，建议绕过本文。为了便于展开，今天先起个头，重点梳理下动态追踪技术的发展简史和目前的生态环境。更加具体详细的内容，会在后续的文章中陆续发表。

严格来说，DTrace这个词本身，已经并不是狭义上基于 Solaris 的那套工具了，而是代表的是后现代操作系统的一整套工具家族和方法论。

#### History of DTrace

>当时 Solaris 操作系统的几个工程师花了几天几夜去排查一个看似非常诡异的线上问题。
开始他们以为是很高级的问题，就特别卖力，结果折腾了几天，最后发现其实是一个非常愚蠢的、某个不起眼的地方的配置问题。
自从那件事情之后，这些工程师就痛定思痛，创造了 DTrace 这样一个非常高级的调试工具，来帮助他们在未来的工作当中避免把过多精力花费在愚蠢问题上面。
毕竟大部分所谓的“诡异问题”其实都是低级问题，属于那种“调不出来很郁闷，调出来了更郁闷”的类型。---《漫谈动态追踪技术》

![](http://riboseyim-qiniu.riboseyim.com/DTrace_History_01.png)

通观DTrace的演变过程，几乎相当于一部现代操作系统系统的发展史，细查起来，极其复杂。但是有两个人非常值得关注，一个是国际级的布道师，一个是国内的代表人物，初学者完全可以通过阅读他们的文章、代码，甚至微博／Twitter动态，了解动态追踪技术的实际应用情况。

**Brendan Gregg**
前SUN性能工程师，最早的DTrace用户，出版了包括《性能之巅》在内的一大批书籍，囊括了性能问题领域的技术、工具、方法论等方方面面。他是动态追踪技术当之无愧的首席布道师，维护的个人博客发布了大量的原创内容，并且持续保持着相当的活跃度。可以作为第一手的学习资料。

[Twitter：](https://twitter.com/brendangregg)
[个人网站：](http://www.brendangregg.com)

**章亦春**
网名 agentzh。开源项目OpenResty创始人，编写了很多 Nginx 的第三方模块， Perl 开源模块，以及最近一些年写的很多 Lua 方面的库。他发表过的[《漫谈动态追踪技术》](https://openresty.org/posts/dynamic-tracing)，是目前唯一由Brendan认证的中文资料，入门首选。另外，他本人也在目前的工作、开源项目运营中大量使用动态追踪技术。
[微博：](http://weibo.com/agentzh)

#### Linux 追踪器选型

动态追踪技术最复杂的地方在于追踪器种类繁多，让人一时无从下手。根据前人的一些经验总结，建议按照以下路径进行选择：

![](http://riboseyim-qiniu.riboseyim.com/DTrace_Linux_Choose.png)

#### 普通模式

适用于：开发者, 系统管理员, DevOps, SRE

**CPU分析**

perf_events的应用很广泛，配合Brendan Gregg老师研究的火焰图工具，可以分析程序在所有代码基的资源消耗，精确定位到函数级。例如：
![火焰图实例](http://riboseyim-qiniu.riboseyim.com/DTrace_Flame_Java_01.png)

**进程追踪**
```
# ./execsnoop
Tracing exec()s. Ctrl-C to end.
   PID   PPID ARGS
 22898  22004 man ls
 22905  22898 preconv -e UTF-8
 22908  22898 pager -s
 22907  22898 nroff -mandoc -rLL=164n -rLT=164n -Tutf8
```

#### HARD模式
适用于：性能或内核工程师

>Understanding all the Linux tracers to make a rational decision between them a huge undertaking.

![](http://riboseyim-qiniu.riboseyim.com/DTrace_Linux_Types.png)

0、dtrace

- 案例：[ipfans:使用dtrace跟踪Python应用](https://ipfans.github.io/2016/09/tracing-python-program-with-dtrace/)

1、ftrace
内核hacker的最爱。已经包含在内核，能够支持 tracepoints, kprobes, and uprobes,
并提供一些能力: 事件追踪, 可选择过滤器和参数; 事件计数和时间采样，内核概览；基于函数的路径追踪。
- [动态追踪技术（三）：Tracing your kernel Functions! | @RiboseYim](https://riboseyim.github.io/2017/04/17/DTrace_FTrace/)

2、perf_events
Linux用户的主要追踪器之一，它的源代码在内核中，通常在一个 linux-tools-common包。

3、eBPF
基于内核的虚拟机

- [动态追踪技术（四）：基于 Linux bcc/BPF 实现 Go 程序动态追踪](https://riboseyim.github.io/2017/06/27/DTrace_bcc/)

4、 SystemTap
最强有力的追踪器。它可以做几乎所有的事情: 分析，打点, kprobes, uprobes (源子 SystemTap), USDT, 内核编程等。

5、LTTng
事件收集器, 优于其它追踪器，支持多种事件类型，包括 USDT。
[The LTTng Documentation v2.9](http://lttng.org/docs/v2.9/#doc-lttng-relayd)
- [Sasha Goldshtein:Tracing Runtime Events in .NET Core on Linux](http://blogs.microsoft.co.il/sasha/2017/03/30/tracing-runtime-events-in-net-core-on-linux/)

6、ktap
一个很有前景的追踪器，基于lua内核虚拟机

7、dtrace4linux
个人开发者业余产出 (Paul Fox) ，将 Sun DTrace迁移到 Linux。

8、OL DTrace
Oracle Linux DTrace，将 DTrace 迁移到Oracle Linux的实现。

9、sysdig
一种新型追踪器， 能够基于类似tcpdump的命令操作 syscall events, 再用lua后处理。

10、strace + gdb
- [strace - trace system calls and signals](http://man7.org/linux/man-pages/man1/strace.1.html)
- [动态追踪技术（二）：strace+gdb 溯源 Nginx 内存溢出异常 ](https://mp.weixin.qq.com/s?__biz=MjM5MTY1MjQ3Nw==&mid=2651939588&idx=1&sn=35f71c5f88d1edf23cb2efc812ab8e6c&chksm=bd578c168a20050041c08618281691f0111f61c789097a69095933057618637fc54817815921#rd)
- [动态追踪技术（四）：基于 Linux bcc/BPF 实现 Go 程序动态追踪](https://riboseyim.github.io/2017/06/27/DTrace_bcc/)
- [time: Sleep requires ~7 syscalls](https://github.com/golang/go/issues/25471#issuecomment-391906366)

#### 火焰图应用案例
- [阮一峰:如何读懂火焰图？](http://www.ruanyifeng.com/blog/2017/09/flame-graph.html)
- [Thayne McCombs:如何使用火焰图来降低服务器负载](https://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=2653548763&idx=1&sn=b88c482a96ad691a2f5b2c01b934ab17&chksm=813a6143b64de8559715fad5e0abece43b64fb1681bfd3f3160ca7ca17c89495cbf64209d026&scene=0&key=d6331003961a2cc469dae6df3bf594604336519731be677350b47d6d9f4909f28d60518b9d1dba75db7af70e0d22b5147382e33be51c54e4d92940bb73afa67b781daf1338e7471339cce5fabc8ba1d2&ascene=0&uin=Mjg2OTA0MDQ4Mg%3D%3D)
- [PROFILING GO APPLICATIONS WITH FLAMEGRAPHS| February 28, 2018](http://brendanjryan.com/golang/profiling/2018/02/28/profiling-go-applications.html)
- [A blazing fast flame graph tool for Node and V8. Used to visualize and explore performance profiling results](https://github.com/mapbox/flamebearer)
- [SRE: Performance Analysis: Tuning Methodology Using a Simple HTTP Webserver In Go |  go tool pprof](https://medium.com/dm03514-tech-blog/sre-performance-analysis-tuning-methodology-using-a-simple-http-webserver-in-go-d475460f27ca)

## 勘误
- No.001 初稿已删除【大家比较熟知的netfilter，就是基于BPF实现的动态编译器】
本来是想表达 iptables 对 bpf 的支持。

## 扩展阅读

#### 动态追踪技术
- [动态追踪技术(一)：DTrace 导论](https://riboseyim.github.io/2016/11/26/DTrace/)
- [动态追踪技术(二)：strace+gdb 溯源 Nginx 内存溢出异常 ](https://mp.weixin.qq.com/s?__biz=MjM5MTY1MjQ3Nw==&mid=2651939588&idx=1&sn=35f71c5f88d1edf23cb2efc812ab8e6c&chksm=bd578c168a20050041c08618281691f0111f61c789097a69095933057618637fc54817815921#rd)
- [动态追踪技术(三)：Tracing Your Kernel Function!](https://riboseyim.github.io/2017/04/17/DTrace_FTrace/)
- [动态追踪技术(四)：基于 Linux bcc/BPF 实现 Go 程序动态追踪](https://riboseyim.github.io/2017/06/27/DTrace_bcc/)
- [动态追踪技术(五)：Welcome DTrace for Linux](https://riboseyim.github.io/2018/02/16/DTrace-Linux/)

#### 《Linux Perf Master》

- https://riboseyim.gitbook.io/perf
- https://www.gitbook.com/book/riboseyim/linux-perf-master/details

![](http://riboseyim-qiniu.riboseyim.com/banner-LPM-201803.png)

#### How Linux Works
- [Linux 性能诊断:负载评估](https://riboseyim.github.io/2017/12/11/Linux-Perf-Load/)
- [Linux 性能诊断:快速检查单](https://riboseyim.github.io/2017/12/11/Linux-Perf-Netflix/)
- [操作系统原理 | How Linux Works（一）：How the Linux Kernel Boots](https://riboseyim.github.io/2017/05/29/Linux-Works/)
- [操作系统原理 | How Linux Works（二）：User Space & RAM](https://riboseyim.github.io/2017/05/29/Linux-Works/)
- [操作系统原理 | How Linux Works（三）：内存管理](https://riboseyim.github.io/2017/12/11/Linux-Works-Memory/)
- [操作系统原理 | How Linux Works（三）：网络管理](https://riboseyim.github.io/2018/01/08/Linux-Works-Network/)

## 参考文献
- [动态追踪技术漫谈 | 章亦春](https://openresty.org/posts/dynamic-tracing/)
- [动态追踪技术（中） - Dtrace、SystemTap、火焰图 | 原创 2016-05-06 章亦春 MacTalk](https://mp.weixin.qq.com/s?__biz=MjM5ODQ2MDIyMA==&amp;mid=2650712266&amp;idx=1&amp;sn=54d909d240eb701ae48467dc798ddc7f&amp;scene=1&amp;srcid=0506Cd7v9OLvaEFRKBTD2ARy%23rd)
- [动态追踪技术-应用性能瓶颈排障利器之火焰图 | 2016-12-01 郑晓川 江凌生 TIGCHAT](https://mp.weixin.qq.com/s/4Y77jUqfgKeBS2hnitrUrg)
- [运维利器：万能的 strace | 2016-05-24 王子勇 高效运维](https://mp.weixin.qq.com/s?__biz=MzA4Nzg5Nzc5OA==&mid=2651659767&idx=1&sn=3c515cb32bcbcafe16c749024d1545ef&scene=1&srcid=0524lBh8JTWhnUmtv72PiQSN%23rd)
- [(推荐)Linux tracing systems & how they fit together | Julia Evans ](https://jvns.ca/blog/2017/07/05/linux-tracing-systems/)
- [Give me 15 minutes and I'll change your view of Linux tracing | Brendan Gregg's Blog ](http://www.brendangregg.com/blog/2016-12-27/linux-tracing-in-15-minutes.html)
- [Where has my disk space gone? Flame graphs for file systems | Brendan Gregg's Blog](http://www.brendangregg.com/blog/2017-02-05/file-system-flame-graph.html)
- [Container Performance Analysis at DockerCon 2017 | Brendan Gregg's Blog](http://www.brendangregg.com/blog/2017-05-15/container-performance-analysis-dockercon-2017.html)
- [(推荐) Julia Evans: Linux tracing systems & how they fit together](https://jvns.ca/blog/2017/07/05/linux-tracing-systems/)
- [使用 dtrace 跟踪 Python 应用 | ipfans's Blog](https://ipfans.github.io/2016/09/tracing-python-program-with-dtrace/)
- [Probing the JVM with BPF/BCC | Sasha Goldshtein](http://blogs.microsoft.co.il/sasha/2016/03/31/probing-the-jvm-with-bpfbcc/)
- [从eBay购物车丢失看处理网络I/O ](http://www.infoq.com/cn/news/2017/07/eBay-shopping-i-o?utm_campaign=infoq_content&utm_source=infoq&utm_medium=feed&utm_term=global)
- [Go’s march to low-latency GC](https://blog.twitch.tv/gos-march-to-low-latency-gc-a6fa96f06eb7)
- [Tracing Runtime Events in .NET Core on Linux | March 30, 2017](http://blogs.microsoft.co.il/sasha/2017/03/30/tracing-runtime-events-in-net-core-on-linux/)
- [案例分享：巧用各种工具提升无源码系统的性能和稳定性 | 原创： 杨振 董建  高可用架构  1月17日](https://mp.weixin.qq.com/s/3UXBEUmI3dMy9DJHs5gsRg)
- [COZ: Finding Code that Counts with Causal Profiling](https://www.sigops.org/s/conferences/sosp/2015/current/2015-Monterey/printable/090-curtsinger.pdf)
