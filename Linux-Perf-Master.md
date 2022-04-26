---
title: Linux 性能诊断高级课程
date: 2016-04-26 12:00:23
tags: [SRE,DevOps,Linux,Engineering]
categories: 出版物
---
## 摘要
- 方法论
- 性能诊断指南
- How Linux Works
- 动态追踪技术
- 案例与实务
- 推荐书单

<!--more-->


#### 方法论

| 方法              | 信息收集 | 观测分析 | 统计分析 | 容量规划 | 调优 | 生命周期分析 | 实验分析 | 假设分析 |     |
| ----------------- | -------- | -------- | -------- | -------- | ---- | ------------ | -------- | -------- | --- |
| 街灯              |          | Yes      |          |          |      |              |          |          |     |
| 随机变动          |          |          |          |          |      |              | Yes      |          |     |
| 责怪他人          |          |          |          |          |      |              |          | Yes      |     |
| Ad Hoc 核对清单法 |          | Yes      |          |          |      |              | Yes      |          |     |
| 问题陈述法        | Yes      |          |          |          |      |              |          |          |     |
| 科学法            |          | Yes      |          |          |      |              |          |          |     |
| 循环诊断法        |          |          |          |          |      | Yes          |          |          |     |
| 工具法            |          | Yes      |          |          |      |              |          |          |     |
| USE法             |          | Yes      |          |          |      |              |          |          |     |
| 工作负载特征归纳  |          | Yes      |          | Yes      |      |              |          |          |     |
| 向下挖掘分析      |          | Yes      |          |          |      |              |          |          |     |
| 延时分析          |          | Yes      |          |          |      |              |          |          |     |
| R 方法            |          | Yes      |          |          |      |              |          |          |     |
| 时间跟踪          |          | Yes      |          |          |      |              |          |          |     |
| 基础线统计        |          | Yes      |          |          |      |              |          |          |     |
| 性能监控          |          | Yes      |          | Yes      |      |              |          |          |     |
| 排队论            |          |          | Yes      | Yes      |      |              |          |          |     |
| 静态性能调整      |          | Yes      |          | Yes      |      |              |          |          |     |
| 缓存调优          |          | Yes      |          |          | Yes  |              |          |          |     |
| 微基准测试        |          |          |          |          |      |              | Yes      |          |     |
| 容量规划          |          |          |          | Yes      | Yes  |              |          |          |     |


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

#### 案例与实务
- [最佳工程实践：Stack Overflow 架构 - 2016 Edition](https://riboseyim.github.io/2016/07/17/OpenSource-StackOverflow/)
- [最佳工程实践：Oracle 数据库迁移割接实践](https://riboseyim.github.io/2016/06/12/Technology-Oracle/)
- [最佳工程实践：基于LVS的AAA负载均衡架构实践](https://riboseyim.github.io/2016/09/01/AAA/)  
- [VIPServer | Facebook Open-sourcing Katran, a scalable network load balancer](https://code.facebook.com/posts/1906146702752923/open-sourcing-katran-a-scalable-network-load-balancer/)

## 推荐书单

#### 电子书《Linux Perf Master》

- https://riboseyim.gitbook.io/perf
- https://www.gitbook.com/book/riboseyim/linux-perf-master/details

![](http://riboseyim-qiniu.riboseyim.com/banner-LPM-201803.png)

#### [读书笔记|《图解性能优化》](https://riboseyim.github.io/2017/10/24/LinuxPerf-Picture/)
- 性能分析的基础:吞吐和响应的区别
- 实际系统中的性能分析
- 性能调优 & 性能测试
- 虚拟化环境下的性能
- 云环境下的性能

#### [《24小时365天不间断服务》](http://www.jianshu.com/p/7a633fdada4f)

#### [《性能之巅》](http://www.jianshu.com/p/7a633fdada4f)

#### 《Google核心技术》


## 快捷方式
```
# 按内存排序，由大到小;rsz为实际内存
ps -e -o 'pid,comm,args,pcpu,rsz,vsz,stime,user,uid' | grep oracle |  sort -nrk5

# 查看打开的文件
lsof

# 查看 Threads
ps m
```

## 参考文献
- [Server-side I/O Performance: Node vs. PHP vs. Java vs. Go](https://www.toptal.com/back-end/server-side-io-performance-node-php-java-go)
- [Linkedin:Real-time distributed tracing for website performance and efficiency optimizations](Real-time distributed tracing for website performance and efficiency optimizations)
 - [推荐书单](http://www.jianshu.com/p/7a633fdada4f)
 - [程序员需要跨过性能这个坎 | 原创 2017-07-13 池建强 MacTalk](https://mp.weixin.qq.com/s?__biz=MjM5ODQ2MDIyMA==&mid=2650713353&idx=1&sn=c2a2b22eafa6be8a18542b6752f8bd24&chksm=bec0635a89b7ea4c7fe653aacb981434fd604665fe6d8d1775ecb169ef3eea43528e464af3de&mpshare=1&scene=1&srcid=0713CHRf2HiItQatKLrXoxp4%23rd)
 - [望闻问切，解决Linux系统性能问题 | 2016-10-10 Brendan Gregg 开发资讯](https://mp.weixin.qq.com/s/qAcr02Hwjf_9_Ks3DwjHmw)
