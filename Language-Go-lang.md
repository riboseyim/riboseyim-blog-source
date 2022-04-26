---
title: Catalog：玩转编程语言 - Golang 专题
date: 2017-05-05 14:56:56
tags: [Golang,DevOps,Developer]
categories: 工程技术
---
## 摘要
- ABC
- 基于 Golang 的应用
- Golang 最佳实践

<!--more-->

#### 围棋十诀

>一、不得贪胜。
二、入界宜缓。
三、攻彼顾我。
四、弃子争先。
五、舍小就大。
六、逢危须弃。
七、慎勿轻速。
八、动须相应。
九、彼强自保。
十、势孤取和。
————（唐）王积薪

## 一、ABC

- [Go by Example](https://gobyexample.com/)
- [Popular sites, blogs and tutorials for learning and mastering Go Language](https://www.cybrhome.com/topic/go-language-tutorials)
- [Learn Go with tests](https://quii.gitbook.io/learn-go-with-tests)

#### 1.1 Install

- [https://go.googlesource.com/go/](https://go.googlesource.com/go/)

```bash
tar -C /usr/local/go-$version -xvf go-$version.tar.gz

cd /usr/local/go-$version/src
./all_bash

#compile,build and test

unset GOROOT
unset GOPATH
unset GOAPPATH

export GOROOT=/usr/local/go
export GOPATH=$GOROOT/bin
export GOAPPATH=$GOROOT/bin/bin
export PATH=$PATH:$GOPATH:$GOAPPATH

```

#### 1.2 Go Version Manage

```go
//atom 代码提示
go get -u github.com/jstemmer/gotags
//源代码目录执行
gotags -tag-relative=true -R=true -sort=true -f="tags" -fields=+l .  

```
- build

```make

## 交叉编译

build-linux:
	export CGO_ENABLED=0 && export GOOS=linux && export GOARCH=amd64 && go build
build-wins:
	export CGO_ENABLED=0 && export GOOS=windows && export GOARCH=amd64 && go build
build-solaris:
  export CGO_ENABLED=0 && export GOOS=solaris && export GOARCH=amd64 && go build

## Go supports Solaris 11 on amd64, but not sparc.To build for sparc you need to use gccgo.

## 压缩
go build -ldflags '-w -s'
```

## 二、基于 Golang 的应用

#### 2.1 [基于Go语言构建RESTful JSON API](https://riboseyim.github.io/2017/05/23/RestfulAPI/)

#### 2.2 [基于Kafka构建事件溯源模式的微服务](https://riboseyim.github.io/2017/06/12/OpenSource-Kafka-Microservice/)
讨论如何借助Kafka实现分布式消息管理，使用事件溯源（Event Sourcing）模式实现原子化数据处理，使用CQRS模式（Command-Query Responsibility Segregation ）实现查询职责分离，使用消费者群组解决单点故障问题，理解分布式协调框架Zookeeper的运行机制。整个应用的代码实现使用Go语言描述。

#### 2.3 [开源技术架构漫谈：应用程序开发中的日志管理](https://riboseyim.github.io/2017/05/24/Log/)

#### 2.4 [网络数据包的捕获、过滤和分析(Packet Capturing)](https://riboseyim.github.io/2017/06/16/Network-Pcap/)
- What is Packet Capturing
- How can it be used
- What is libpcap
- What is tcpdump & winpcap & snoop
- What is BPF
- What is gopacket

#### 2.5 [数据可视化（五）基于网络爬虫制作可视化图表](https://riboseyim.github.io/2017/05/12/Visualization-Charts/)
- 基于网络爬虫的可视化图表:golang,goquery
- 案例：最近十年全国彩票销售变化情况
- 案例：中国科学院院士分布
- 数据可视化技术方案:基于 SVG (D3、Raphael)、基于 Canvas（Echarts）

## 三、Golang 最佳实践

#### 3.1 Grammar Tips & Simple Demo
- [Port Forwarding with Go (zupzup.org)](http://crwd.fr/2oDS2RF)
- [A guide to understanding HTTP Request handling and processing in Go](https://lanreadelowo.com/blog/2017/04/03/http-in-go/)
- [Understand Go pointers in less than 800 words ](https://dave.cheney.net/2017/04/26/understand-go-pointers-in-less-than-800-words-or-your-money-back)
- [there-is-no-pass-by-reference-in-go](https://dave.cheney.net/2017/04/29/there-is-no-pass-by-reference-in-go)
- [让go get显示进度](http://life.leanote.com/post/%E8%AE%A9-go-get-%E6%98%BE%E7%A4%BA%E8%BF%9B%E5%BA%A6)
- [exec.Command()实时输出](http://life.leanote.com/post/golang-exec%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4%E5%AE%9E%E6%97%B6%E8%BE%93%E5%87%BA)
- [Building a Worker Pool in Golang | Dynamically scalable queue consumer](https://geeks.uniplaces.com/building-a-worker-pool-in-golang-1e6c0fdfd78c)
- [How did I improve latency by 700% using sync.Pool](http://www.akshaydeo.com/blog/2017/12/23/How-did-I-improve-latency-by-700-percent-using-syncPool/)

The interface is a tool so that you can wire dependencies just by their behaviour, not by how they implement that behaviour.
- [Golang Interfaces](https://medium.com/full-stack-tips/golang-interfaces-26033f6a8858)
- [用 Go Plugin 构建模块化系统](http://yoursite.com/build-module-system-use-go-plugin/)

#### 3.2 Specialist: Architecture

- [Using Einstein Vision Within Golang （Waiting）](https://developer.salesforce.com/blogs/developer-relations/2017/05/using-einstein-vision-within-golang.html)
By Rajdeep Dua | Published: May 5, 2017
Einstein Vision is a service that helps you build smarter applications by using deep learning to automatically recognize images. It provides an API that lets you use image recognition to build AI-enabled apps.

- [Trying Clean Architecture on Golang: Independent, Testable , and Clean](https://hackernoon.com/golang-clean-archithecture-efd6d7c43047)
By Iman TumorangFollow | Passionate and Curious Learner in Software Engineering

- [ 【翻译】生活在没有泛型的Go语言世界里 | Cholerae](http://cholerae.com/2015/01/02/%E3%80%90%E7%BF%BB%E8%AF%91%E3%80%91%E7%94%9F%E6%B4%BB%E5%9C%A8%E6%B2%A1%E6%9C%89%E6%B3%9B%E5%9E%8B%E7%9A%84Go%E8%AF%AD%E8%A8%80%E4%B8%96%E7%95%8C%E9%87%8C/)
- [5 Reasons Why We switched from Python To Go](https://hackernoon.com/5-reasons-why-we-switched-from-python-to-go-4414d5f42690)

#### 3.3 Go Repository
- [Walk through JSON with Go](https://github.com/romanyx/jwalk)
- [Using job queues in Go for resilient systems](https://blog.gojekengineering.com/using-job-queues-in-go-for-resilient-systems-638526316e7e)
- [mux router | Go Router Repository](http://www.gorillatoolkit.org/pkg/mux)
- [httprouter | Go Router Repository](https://github.com/julienschmidt/httprouter)
- [Go packages in R packages](https://romain.rbind.io/blog/2017/06/09/go-packages-in-r-packages/)

- [NanoDano:Making Tor HTTP Requests with Go | Socket Proxy](http://www.devdungeon.com/content/making-tor-http-requests-go)
- [vlan-nats](https://github.com/rapidloop/vlan-nats)
- [riot：Go Open Source, Distributed, Simple and efficient Search Engine](https://github.com/go-ego/riot)
- [K6: A modern load testing tool, using Go and JavaScript](https://k6.io)
- [internationalization-i18n-go](https://phraseapp.com/blog/posts/internationalization-i18n-go/)

#### [goproxy](https://github.com/snail007/goproxy/blob/master/README_ZH.md)
golang实现的高性能http,https,websocket,tcp,udp,socks5代理服务器,支持正向代理、反向代理、透明代理、内网穿透、TCP/UDP端口映射、SSH中转，TLS加密传输，协议转换

- [Advanced Forwarding with Go github.com/elazarl/goproxy](https://github.com/elazarl/goproxy)
- [Encrypted reverse proxy in Go.](https://github.com/wybiral/reverseproxy)
- [TavenLi/port-forward](https://git.oschina.net/tavenli/port-forward)

- 爬虫(简易型) ：https://github.com/lealife/leacrawler

#### 3.3 Business Product List
- [InfluxDB](https://www.influxdata.com):时间序列数据库，实时数据场景
- [Marketstore | The Financial Time Series Database](https://hackernoon.com/marketstore-the-financial-time-series-database-is-now-open-source-fd04343f439)
- [File Manager | Web Browser](https://filebrowser.github.io/)

## Books

#### [《Introducing Go: Build Reliable, Scalable Programs》](#)

- 特点：精简
- [预览链接](https://www.amazon.com/Introducing-Go-Reliable-Scalable-Programs-ebook/dp/B01AB3G496/ref=mt_kindle?_encoding=UTF8&me=&qid=1529338751)
- Others [《The Little Go Book》](http://openmymind.net/The-Little-Go-Book/)

![](http://riboseyim-qiniu.riboseyim.com/Books_Introducing_Go.png)

#### 《The Go Programming Language》

- 特点：基础全面
- [预览链接](https://www.amazon.com/gp/product/0134190440/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0134190440&linkCode=as2&tag=henrmota-20&linkId=c841528d2e6d85f2bad943d19e4527e0)

![](http://riboseyim-qiniu.riboseyim.com/Book_Go_Programming_Language.png)

#### [《Go Web Programming》](#)

- 特点：Web Application
- [预览链接](https://www.amazon.com/gp/product/1617292567/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1617292567&linkCode=as2&tag=henrmota-20&linkId=f29795534888ac4c517e19b8afb0d6a5)

![](http://riboseyim-qiniu.riboseyim.com/Books_Go_Web_Programming.png)

#### Concurrency in Go: Tools and Techniques for Developers

- 特点：Concurrency
- [预览链接](https://www.amazon.com/gp/product/1491941197/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1491941197&linkCode=as2&tag=henrmota-20&linkId=4b443a84ffbac9cc6101b2f492ec9e13)

![](http://riboseyim-qiniu.riboseyim.com/Books_Concurrency_In_Go.png)

#### [《Go Programming Blueprints》](#)

- 特点：covers a lot of topics，such as web services,command-line tools,microservices and app deployment.
- [预览链接](https://www.amazon.com/gp/product/1786468948/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1786468948&linkCode=as2&tag=henrmota-20&linkId=ef8749456ce088e32381286f0743d251)


![](http://riboseyim-qiniu.riboseyim.com/Books_Go_Programming_Blueprints.png)

#### Others

- [Karl Seguin](http://www.goodreads.com/author/show/1524282.Karl_Seguin)

book: https://github.com/thewhitetulip/web-dev-golang-anti-textbook/
youtube series: https://www.youtube.com/playlist?list=PL41psiCma00wgiTKkAZwJiwtLTdcyEyc4
code: http://github.com/thewhitetulip/Tasks

## Quickstart

```go
//通过channel 实现协程间通信
// https://golangcaff.com/docs/the-way-to-go/142-covariance-channel/130
import (
	"fmt"
	"time"
)
func worker(done chan bool) {
	time.Sleep(time.Second)
	// 通知任务已完成
	done <- true
}
func main() {
	done := make(chan bool, 1)
	go worker(done)
	// 等待任务完成
	<-done
}

```

## Tips

-  一些 package时候的会由于众所周知的原因而无法下载
```go
unrecognized import path "golang.org/x/sys/unix"
cd $GOPATH/src/golang.org/x/
git clone --depth=1 https://github.com/golang/xxx.git
```

- [error handling in Go: more expressive syntax](http://alehander42.me/go)
- [(Slides) High Performance Go | Infoq.com ](https://www.infoq.com/presentations/go-programming-language)

## Security Resources

- [Awesome golang Security resources](https://github.com/guardrailsio/awesome-golang-security)

## 扩展阅读

- [玩转编程语言系列](https://riboseyim.github.io/2017/05/26/Language/)

## 参考文献

- [GitBook:《深入解析Go》](https://www.gitbook.com/book/tiancaiamao/go-internals/details)
- [ipfans:使用vendor管理Golang项目依赖](https://ipfans.github.io/2016/01/golang-vendor/)
- [(推荐)InfoQ:goroutine背后的系统知识](http://www.infoq.com/cn/articles/knowledge-behind-goroutine)
- [Qu Xiao:Goroutine + Channel 实践](http://studygolang.com/articles/2423)
- [A Dive Into the `fmt` Package](https://blog.gopheracademy.com/advent-2018/fmt/)
- [The Relationship Between Interfaces and Reflection](https://blog.gopheracademy.com/advent-2018/interfaces-and-reflect/)
- [Using Go in Devops](https://blog.gopheracademy.com/advent-2018/go-devops/)
