---
title: How Linux Works：Shell 编程
date: 2019-05-12 16:42:47
tags: [Linux,DevOps]
categories: 工程技术
---
## 摘要
- Overview
- BASIC
- key words:echo、grep、awk、xargs
- functions & parameters
- user environment: crontabs
- concurrent: threads
- Application

<!--more-->

## Overview


### When to Use Shell Scripts


## Shell Script Basics

![shell-startup](https://blog.flowblok.id.au/static/images/shell-startup.png)

### User Environments

### Key Words

#### Echo
- [Command-line-text-processing](https://github.com/learnbyexample/Command-line-text-processing/blob/master/gnu_awk.md)

```bash
$ echo 'foo:123:bar:789' | awk -F: '{print $2,$3}'
123 bar

$ # second field where input field separator is :
$ echo 'foo:123:bar:789' | awk -F: '{print $2}'
123

$ # last field
$ echo 'foo:123:bar:789' | awk -F: '{print $NF}'
789

$ # first and last field
$ # note the use of , and space between output fields
$ echo 'foo:123:bar:789' | awk -F: '{print $1, $NF}'
foo 789

$ # second last field
$ echo 'foo:123:bar:789' | awk -F: '{print $(NF-1)}'
bar

$ # use quotes to avoid clashes with shell special characters
$ echo 'one;two;three;four' | awk -F';' '{print $3}'
three

$ echo 'Sample123string54with908numbers' | awk -F'[0-9]+' '{print $2}'
string
```


#### Grep


#### AWK


#### Xargs


## Application

#### 文本查找

```bash
$ grep -rn "Hello" *
```


## Template


## 扩展阅读

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
- [AWK in 20 minutes](https://ferd.ca/awk-in-20-minutes.html)
- [Ten Things I Wish I’d Known About bash](https://zwischenzugs.com/2018/01/06/ten-things-i-wish-id-known-about-bash/)
- [Echo Command in Linux with Examples](https://linuxize.com/post/echo-command-in-linux-with-examples/)
- [Shell startup scripts](https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html)
- [Introduction to Linux IO, Standard Streams, and Redirection.](https://www.putorius.net/linux-io-file-descriptors-and-redirection.html)
