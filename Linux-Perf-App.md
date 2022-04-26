---
title: Linux 性能诊断：Web Server
date: 2018-06-11 14:57:15
tags: [SRE,DevOps,Linux]
categories: 工程技术
---
## 摘要

Web Server 性能分析

- Apache vs Nginx
- JVM vs Go

<!--more-->

## 问题

```bash
#
[Wed Jul 25 22:19:05 2018] [error] server reached MaxClients setting, consider raising the MaxClients setting

```

```perl
## 持续输出进程数
$ watch "pgrep httpd | wc -l"
Every 2.0s: pgrep httpd | wc -l     Thu Jul 26 21:12:12 2018
42
$ ps  -ylC httpd --sort:rss
S   UID    PID   PPID  C PRI  NI   RSS    SZ WCHAN  TTY          TIME CMD
S   501  37620      1  0  80   0  1268 11253 poll_s ?        00:14:17 httpd
S   501  37622  37620  0  80   0  1708 11320 inet_c ?        00:00:00 httpd
S   501  37623  37620  0  80   0  1708 11320 inet_c ?        00:00:00 httpd
S   501  37624  37620  0  80   0  1708 11320 inet_c ?        00:00:00 httpd
S   501  37625  37620  0  80   0  1708 11320 inet_c ?        00:00:00 httpd
.....
## 当前连接数
$ netstat -an | grep :2003 | wc -l
172
# 累计消耗内存（M）
$ ps aux|grep -v grep|awk '/httpd/{sum+=$6;n++};END{print sum/1024}'
# 平均每个进程消耗内存（M）
$ ps aux|grep -v grep|awk '/httpd/{sum+=$6;n++};END{print sum/n/1024}'
```

#### Apache Server MPM

>问题：为什么 Apache HTTP Server 启动有多个进程

多处理模块(Multi -Processing Modules，MPM)

```c
$ ./httpd -V
Server version: Apache/2.2.31 (Unix)
......
Architecture:   64-bit
Server MPM:     Prefork
  threaded:     no
    forked:     yes (variable process count)
Server compiled with....
```

- Prefork 工作模式
```c
//httpd.conf 默认配置
<IfModule prefork.c>       
StartServers      8          # 服务初始化的工作进程数（work process）
MinSpareServers    5         # 保持的最少空闲进程数
MaxSpareServers     20       # 保持的最大空闲进程数
ServerLimit      256         # 保持的最大活动进程数，设定MaxClients的上限值
MaxClients       256         # 最大并发连接数
MaxRequestsPerChild  4000    # 每个子进程在生命周期能服务的最大请求数,即控制每个进程在处理了多少次请求之后自动销毁
</IfModule>
```

- Worker 工作模式
```c
//httpd.conf 默认配置
<IfModule worker.c>
StartServers       4         # 初始化的子进程数
MaxClients        300        # 并发请求最大数
MinSpareThreads     25         # 最小空闲线程数total=所有进程的线程数加起来
MaxSpareThreads     75         # 最大空闲线程数
ThreadsPerChild     25         # 每个子进程可生成的线程数
MaxRequestsPerChild   100      # 每个子进程可服务的最大请求数,0表示不限制,建议设置为非0
</IfModule>
```
- Event 工作模式

## Apache vs. Nginx

- [How to Monitor Nginx Performance Using Netdata on CentOS 7](https://www.tecmint.com/monitor-nginx-performance-using-netdata-on-centos-7/)

## JVM vs. Go App

- [Go memory ballast: How I learned to stop worrying and love the heap](https://blog.twitch.tv/go-memory-ballast-how-i-learnt-to-stop-worrying-and-love-the-heap-26c2462549a2)

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

#### 案例与实务
- [最佳工程实践：Stack Overflow 架构 - 2016 Edition](https://riboseyim.github.io/2016/07/17/OpenSource-StackOverflow/)
- [最佳工程实践：Oracle 数据库迁移割接实践](https://riboseyim.github.io/2016/06/12/Technology-Oracle/)
- [最佳工程实践：基于LVS的AAA负载均衡架构实践](https://riboseyim.github.io/2016/09/01/AAA/)  
- [VIPServer | Facebook Open-sourcing Katran, a scalable network load balancer](https://code.facebook.com/posts/1906146702752923/open-sourcing-katran-a-scalable-network-load-balancer/)

## 参考文献
- [Java 应用性能调优实践](https://www.ibm.com/developerworks/cn/java/j-lo-performance-tuning-practice/index.html)
- [动态追踪技术-应用性能瓶颈排障利器之火焰图](https://mp.weixin.qq.com/s/4Y77jUqfgKeBS2hnitrUrg)
- [Apache的三种MPM模式比较：prefork，worker，event](http://blog.jobbole.com/91920/)
- [Apache HTTP Server 中prefork和worker工作模式（二）](http://blog.51cto.com/skypegnu1/1532333)
- [Sites are very slow. Apache shows: server reached MaxClients setting on Plesk for Linux](https://support.plesk.com/hc/en-us/articles/213389769-Sites-are-very-slow-Apache-shows-server-reached-MaxClients-setting-on-Plesk-for-Linux)
