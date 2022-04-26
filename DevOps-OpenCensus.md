---
title: DevOps 漫谈：开源分布式跟踪系统 OpenCensus
date: 2018-04-27 10:33:09
tags: [Linux,OpenSource,DevOps,架构师]
categories: 工程技术
---
## 摘要
- Distributed Tracing and Monitoring System
- OpenCensus: A framework for distributed tracing
- OpenCensus Principle: data structure 、Context

<!--more-->

![](http://riboseyim-qiniu.riboseyim.com/DTM-OpenCensus-Theme.png)

This article is part of an **Distributed Tracing and Monitoring System** tutorial series. Make sure to check out my other articles as well:

- [DevOps 漫谈：开源分布式跟踪系统 OpenCensus](https://riboseyim.github.io/2018/04/27/DevOps-OpenCensus)
- [DevOps 漫谈：分布式追踪系统标准体系](https://riboseyim.github.io/2018/05/18/DevOps-OpenTracing/)

## 绪论

随着互联网技术的高速发展，以往单应用的服务架构已经很难处理如山洪般增长的信息数据，随着云计算技术的大规模应用，以微服务、RESTful 为代表的各种软件架构广泛应用，跨团队、跨编程语言的大规模分布式系统也越来越多。相对而言，现在要理解系统行为，追踪诊断性能问题会复杂得多。

在单应用环境下，业务都在同一个服务器上，如果出现错误和异常只需要盯住一个点，就可以快速定位和处理问题；但是在微服务的架构下，功能模块天然是分布式部署运行的，前后台的业务流会经过很多个微服务的处理和传递，就连日志监控都会成为一个大问题（日志分散在多个服务器、无状态服务下如何查看业务流的处理顺序等），更不要说服务之间还有复杂的交互关系。

用户的一个请求在系统中会经过多个子系统（或者多个微服务）的处理，而且是发生在不同机器甚至是不同集群，当发生异常时需要快速发现问题，并准确定位到是哪个环节出了问题。对系统行为进行跟踪必须持续进行，因为异常的发生是无法预料的，有些甚至难以重现。跟踪需要无所不在，否则可能会遗漏某些重要的故障点。

为了解决上述问题，分布式跟踪系统 —— 一种帮助理解分布式系统行为、帮助分析性能问题的工具应运而生。

![](http://riboseyim-qiniu.riboseyim.com/DTM-OpenCensus-Micro-1.png)

![](http://riboseyim-qiniu.riboseyim.com/DTM-OpenCensus-Micro-2.png)


## 第一部分 Google Dapper : Distributed Tracing and Monitoring System

>Modern Internet services are often implemented as complex, large-scale distributed systems.These applications are constructed from collections of software modules that may be developed by different teams, perhaps in different programming languages, and could span many thousands of machines across multiple physical facilities. Tools that aid in understanding system behavior and reasoning about performance issues are invaluable in such an environment.

在分布式追踪领域著名的论文[《Dapper, a Large-Scale Distributed Systems Tracing Infrastructure|Google Technical Report dapper-2010-1, April 2010》](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/papers/dapper-2010-1.pdf) Google 工程师提出了关于分布式跟踪系统的一些重要概念：

- Annotation-based，基于标注或植入点(埋点)
在应用程序或中间件中明确定义全局标注（Annotation），一个特殊的ID，通过这个 ID 连接每一条请求记录。当然，这需要代码植入，在生产环境中可以通过一个通用组件开放给开发人员。

- 跟踪树（trace tree）和 span
在 Dapper 跟踪树中，基本单元是树节点（分配 spanid）。节点之间通过连线表示父子关系，通过 parentId 和 spanId 把所有的关系串联起来，实现记录业务流的作用。

![](http://riboseyim-qiniu.riboseyim.com/DTM-Dapper-TraceTree-Span.png)

## 第二部分 OpenCensus: A framework for distributed tracing

>OpenCensus is a framework for stats collection and distributed tracing.

Google Dapper 的定位更准确的说是分析系统，并不能解决从生产服务中提取数据的难题，OpenCensus 项目为此提供了解决方案。

![](http://riboseyim-qiniu.riboseyim.com/DTM-OpenCensus-Logo.png)

OpenCensus 项目是 Google 开源的一个用来收集和追踪应用指标的第三方库。OpenCensus 能够提供了一套统一的测量工具：跨服务捕获跟踪跨度（span）、应用级别指标以及来自其他应用的元数据（例如日志）。OpenCensus 有如下一些主要特点：
- 标准通信协议和一致的 API ：用于处理 metric 和 trace
- 多语言库，包括Java，C++，Go，.Net，[Python](https://github.com/census-instrumentation/opencensus-python)，PHP，Node.js，Erlang 和 Ruby
- 与 RPC 框架的集成，可以提供开箱即用的追踪和指标。
- 集成的存储和分析工具
- 完全开源，支持第三方集成和输出的插件化
- 不需要额外的服务器或守护进程来支持 OpenCensus
- In process debugging：一个可选的代理程序，用于在目标主机上显示请求和指标数据

![](http://riboseyim-qiniu.riboseyim.com/DTM-OpenCensus-Language.png)

## OpenCensus Concepts

#### Tags | 标签
OpenCensus 允许系统在记录时将度量与维度相关联。记录的数据使我们能够从各种不同的角度分析测量结果，即使在高度互连和复杂的系统中也能够应付。
标签以键值对的形式在上下文中传递，并且允许在当前上下文中添加或修改。

```go
ctx, err = tag.New(ctx,
	tag.Insert(osKey, "macOS-10.12.5"),
	tag.Upsert(userIDKey, "cde36753ed"),
)
```

#### Stats | 统计
Stats 收集库和应用程序记录的测量结果，汇总、导出统计数据。为了实现低开销（ a low-overhead framework even if instrumentation is always enabled ），数据点记录和数据聚合是分离的，OpenCensus 统计收集分两个阶段进行：
- 测量的定义和数据点的记录 | Definition of measures and recording of data points
- 视图的定义和记录数据的聚合 | Definition of views and aggregation of the recorded data

#### Recording | 记录
量度 (Measurements) 是与测量相关联的数据点。(Measurements are data points associated with a measure.)
记录 (Recording) 利用上下文中所提供的标签隐式地标记量度集合。(Recording implicitly tags the set of Measurements with the tags from the provided context.)

```go
stats.Record(ctx, videoSize.M(102478))
```

#### Trace | 跟踪
Trace 是嵌套 Span (跨度)的集合。Trace 包括单个用户请求的处理进度，直到用户请求得到响应。Trace 通常跨越分布式系统中的多个节点。跟踪由 TraceId 唯一标识， Trace 中的所有 Span 都具有相同的 TraceId 。

一个 Span 代表一个操作或一个工作单位。多个 Span 可以是“Trace”的一部分，它代表跨多个进程/节点的执行路径（通常是分布式的）。同一轨迹内的 Span 具有相同的 TraceId。

```go
ctx, span := trace.StartSpan(ctx, "your function name")
defer span.End()
```

#### 视图 | Views
视图用于聚合测量结果。你可以把它们看作是对记录数据点（量度）集合的查询。
视图包括两个部分：分组标签（group by）和聚合类型（aggregation type）。
目前 OpenCensus  ( Golang API ) 支持三种类型的聚合：
- CountAggregation 用于记录被抽样的次数；
- DistributionAggregation 用于提供包含一系列样本值的直方图；
- SumAggregation 用于汇总所有样本值。

```go
distAgg := view.Distribution(0, 1<<32, 2<<32, 3<<32)
countAgg := view.Count()
sumAgg := view.Sum()
```

示例：创建一个视图（ DistributionAggregation，指标 “videoSize” ）
```go
if err := view.Register(&view.View{
	Name:        "my.org/video_size_distribution",
	Description: "distribution of processed video size over time",
	Measure:     videoSize,
	Aggregation: view.Distribution(0, 1<<32, 2<<32, 3<<32),
}); err != nil {
	log.Fatalf("Failed to subscribe to view: %v", err)
}
```

#### Introspection | 内省
OpenCensus 提供在线仪表板，显示进程中的诊断数据。这些页面被称为 z-pages ，它们有助于了解如何查看来自特定进程的数据，而不必依赖任何度量收集器或分布式跟踪后端。

![](http://riboseyim-qiniu.riboseyim.com/DTM-OpenCensus-traceZ.png)

## OpenCensus Examples

#### 创建指标

- 定义指标类型
- 定义显示方式

Track Metrics 一般需要考虑服务负载（Server Load）、响应时间（Response Time）、误码率(Error Rates)等。

实例：
- [opencensus-go-examples-helloworld](https://github.com/census-instrumentation/opencensus-go/blob/master/examples/http/helloworld_server/main.go)
- [opencensus-java-examples](https://github.com/census-instrumentation/opencensus-java)
- [“Hello, world!” for web servers in Go with OpenCensus](https://medium.com/@orijtech/hello-world-for-web-servers-in-go-with-opencensus-29955b3f02c6)

```go
import (
  "go.opencensus.io/stats"
  "go.opencensus.io/tag"
  "go.opencensus.io/stats/view"
)

var (
  requestCounter             *stats.Float64Measure
  codeKey                    tag.Key
  DefaultLatencyDistribution = view.DistributionAggregation{0, 1, 2, 3, 4, 5, 6, 8, 10, 13, 16, 20, 25, 30, 40, 50, 65, 80, 100, 130, 160, 200, 250, 300, 400, 500, 650, 800, 1000, 2000, 5000, 10000, 20000, 50000, 100000}
)
	codeKey, _ = tag.NewKey("banias/keys/code")

  requestCounter, _ = stats.Float64("banias/measures/request_count", "Count of HTTP requests processed", stats.UnitNone)
	view.Subscribe(
		&view.View{
			Name:        "request_count",
			Description: "Count of HTTP requests processed",
			TagKeys:     []tag.Key{codeKey},
			Measure:     requestCounter,
			Aggregation: view.CountAggregation{},
		})

    // requestlatency .....

	view.SetReportingPeriod(1 * time.Second)

```

#### 收集指标数据

- Call the Record method

```go
// Go Code Example
// 说明：defer 用于资源的释放，会在函数返回之前进行调用。
// 如果有多个 defer表达式，调用顺序类似于栈，越后面的 defer 表达式越先被调用。
func (c *Collector) Collect(ctx *fasthttp.RequestCtx) {

  defer func(begin time.Time) {
      responseTime := float64(time.Since(begin).Nanoseconds() / 1000)
      occtx, _ := tag.New(context.Background(), tag.Insert(codeKey, strconv.Itoa(ctx.Response.StatusCode())), )

      stats.Record(occtx, requestCounter.M(1))
      stats.Record(occtx, requestlatency.M(responseTime))

    }(time.Now())

    /*do some stuff */

}
```

#### 第三方数据接口 | Exporter

OpenCensus 是独立于供应商的。OpenCensus 收集和跟踪的应用指标可以在本地显示，也可将其发送到第三方分析工具或监控系统实现可视化，例如：

- [Prometheus|普罗米修斯](https://prometheus.io)
- [Stackdriver|适用于 Google Cloud Platform 与 AWS 应用的监控、日志记录和诊断工具](https://cloud.google.com/stackdriver/) | [示例](https://medium.com/@orijtech/cloud-spanner-instrumented-by-opencensus-and-exported-to-stackdriver-6ed61ed6ab4e)
- [Zipkin](https://zipkin.io)
- [OpenCensus Tracing with Uber’s Jaeger project](https://medium.com/@DazWilkin/opencensus-tracing-w-jaeger-2ada1656a2bf)

```go
  import (
  	 "go.opencensus.io/exporter/prometheus"
	   "go.opencensus.io/exporter/stackdriver"
     openzipkin "github.com/openzipkin/zipkin-go"
   	 "go.opencensus.io/exporter/zipkin"
     xray "github.com/census-instrumentation/opencensus-go-exporter-aws"
   	 "go.opencensus.io/trace"
	   "go.opencensus.io/stats/view"
 )

	// Opention: Export to Prometheus Monitoring.
  Exporter, err := prometheus.NewExporter(prometheus.Options{})
	if err != nil {
		logger.Error("Error creating prometheus exporter  ", zap.Error(err))
	}
	view.RegisterExporter(pExporter)


  // Opention: Export to Stackdriver Monitoring.
	sExporter, err := stackdriver.NewExporter(stackdriver.Options{ProjectID: config.ProjectID})
	if err != nil {
		logger.Error("Error creating stackdriver exporter  ", zap.Error(err))
	}
	view.RegisterExporter(sExporter)

  // Opention: Export to Zipkin Monitoring.
	localEndpoint, err := openzipkin.NewEndpoint("service-A", "127.0.1.1:8080")
	reporter := http.NewReporter("http://127.0.1.110:9411/api/v2/spans")
	defer reporter.Close()
	exporter := zipkin.NewExporter(reporter, localEndpoint)
	trace.RegisterExporter(exporter)

  // Opention: Export to AWS X-Ray
	xe, err := xray.NewExporter(xray.WithVersion("latest"))
	if err != nil {
		log.Fatalf("Failed to create AWS X-Ray exporter: %v", err)
	}
	trace.RegisterExporter(xe)

```

#### 数据可视化

- 函数内容为空（微秒级）

![](http://riboseyim-qiniu.riboseyim.com/zipkin-%E7%A9%BA%E6%96%B9%E6%B3%95.png)

- 串行调用函数方法，内容包括网络访问和持久化操作（毫秒级）

![](http://riboseyim-qiniu.riboseyim.com/zipkin-%E4%B8%B2%E8%A1%8C.png)

- 并行调用函数方法（Go routine），内容与上同

![](http://riboseyim-qiniu.riboseyim.com/zipkin-%E5%B9%B6%E8%A1%8C.png)

- 串行/并行混合调用

```go
go go_ping(ctx, "192.168.213.128", 2, time.Second*3, false)
go go_ping(ctx, "192.168.213.128", 2, time.Second*3, false)
go go_ping(ctx, "192.168.213.129", 2, time.Second*5, false)
go_ping(ctx, "192.168.213.128", 2, time.Second*3, false)
go_ping(ctx, "192.168.213.128", 2, time.Second*3, false)
go_ping(ctx, "192.168.213.128", 2, time.Second*3, false)
```

![](http://riboseyim-qiniu.riboseyim.com/zipkin-%E6%B7%B7%E5%90%88.png)

- 多服务调用

![OpenZipkin-Twitter](http://riboseyim-qiniu.riboseyim.com/DTM-Zipkin-Twitter.png)


## 第三部分：OpenCensus Principle | 工作原理 (待续)

#### 数据结构

Span 共有属性：
- TraceId
- SpanId
- Start Time
- End Time
- Status

Span 可选属性：
- Parent SpanId
- Remote Parent
- Attributes
- Annotations
- Message Events
- Links

```go
//go.opencensus.io/trace.go

type Span struct {
	// data contains information recorded about the span.
	//
	// It will be non-nil if we are exporting the span or recording events for it.
	// Otherwise, data is nil, and the Span is simply a carrier for the
	// SpanContext, so that the trace ID is propagated.
	data        *SpanData
   // protects the contents of *data (but not the pointer value.)
	mu          sync.Mutex
	spanContext SpanContext
	// spanStore is the spanStore this span belongs to, if any, otherwise it is nil.
	*spanStore

	endOnce sync.Once

	executionTracerTaskEnd func()
}

type SpanContext struct {
	TraceID      TraceID
	SpanID       SpanID
	TraceOptions TraceOptions
}

```

![](http://riboseyim-qiniu.riboseyim.com/OpenCensus-DataModel-Base.png)

#### 上下文 Context

![](http://riboseyim-qiniu.riboseyim.com/DTM-Context.png)

上下文 Context 按照树型关系构建。以 Golang 为例，创建 Context 树第一步就是通过 context.Background() 得到根节点，再由 WithCancel()、WithTimeout() 等函数创建其它的子节点，孙节点。子节点从父节点复制得到，在子节点也可以设定新的状态值，如此就可以使元数据在子节点之间层层传递。

```go

func Background() Context

func WithCancel(parent Context) (ctx Context, cancel CancelFunc)

func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)

func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)

func WithValue(parent Context, key interface{}, val interface{}) Context

```


- gRPC Client

```go
conn, err := grpc.Dial(address, grpc.WithStatsHandler(&ocgrpc.ClientHandler{}), grpc.WithInsecure())
defer conn.Close()

c := pb.NewGreeterClient(conn)

...

for {
  r, err := c.SayHello(context.Background(), &pb.HelloRequest{Name: name})
}
```

- gRPC Server

```go
// SayHello implements helloworld.GreeterServer
func (s *server) SayHello(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
	ctx, span := trace.StartSpan(ctx, "sleep")
	time.Sleep(time.Duration(rand.Float64() * float64(time.Second)))
	span.End()
	return &pb.HelloReply{Message: "Hello " + in.Name}, nil
}
```

- **go.opencensus.io/trace.go** 源码

```go
//go.opencensus.io/trace.go

// StartSpan starts a new child span of the current span in the context. If
// there is no span in the context, creates a new trace and span.
func StartSpan(ctx context.Context, name string, o ...StartOption) (context.Context, *Span) {
	var opts StartOptions
	var parent SpanContext
	if p := FromContext(ctx); p != nil {
		parent = p.spanContext
	}
	for _, op := range o {
		op(&opts)
	}
	span := startSpanInternal(name, parent != SpanContext{}, parent, false, opts)

	ctx, end := startExecutionTracerTask(ctx, name)
	span.executionTracerTaskEnd = end
	return NewContext(ctx, span), span
}

// FromContext returns the Span stored in a context, or nil if there isn't one.
func FromContext(ctx context.Context) *Span {
	s, _ := ctx.Value(contextKey{}).(*Span)
	return s
}
```

#### 视图

- 注册-订阅 模式

视图注册之后开始收集给定的数据。一旦该视图被订阅，它就向已注册的 Exporter 报送数据。

```go
// source code : go.opencensus.io/view/worker.go

// Register begins collecting data for the given views.
// Once a view is subscribed, it reports data to the registered exporters.
func Register(views ...*View) error {
	for _, v := range views {
		if err := v.canonicalize(); err != nil {
			return err
		}
	}
	req := &registerViewReq{
		views: views,
		err:   make(chan error),
	}
	defaultWorker.c <- req
	return <-req.err
}
```

#### Exporter

```go
// source: go.opencensus.io/trace/export.go

// SpanData contains all the information collected by a Span.
type SpanData struct {
	SpanContext
	ParentSpanID SpanID
	SpanKind     int
	Name         string
	StartTime    time.Time
	// The wall clock time of EndTime will be adjusted to always be offset
	// from StartTime by the duration of the span.
	EndTime time.Time
	// The values of Attributes each have type string, bool, or int64.
	Attributes    map[string]interface{}
	Annotations   []Annotation
	MessageEvents []MessageEvent
	Status
	Links           []Link
	HasRemoteParent bool
}
```

## OpenCensu 资讯

#### [Microsoft joins the OpenCensus project | June 13,2018](https://open.microsoft.com/2018/06/13/microsoft-joins-the-opencensus-project/)

>We are happy to announce that Microsoft is joining the open source OpenCensus project, originally initiated and shepherded by Google, and we are excited to help it achieve the goal of “a single distribution of libraries for metrics and distributed tracing with minimal overhead.”

微软宣布加入开源项目 OpenCensus —— 最初是由 Google 发起和主导，旨在建立一个低开销的分布式追踪和度量库。

现代基于云的应用程序通常是分布式的, 需要专门的监测和追踪技术来跟踪定位故障和性能问题。Azure 应用程序内置 [Application Map，应用程序映射|会审分布式应用程序](https://docs.microsoft.com/zh-cn/azure/application-insights/app-insights-app-map) 和 [End-to-End Transaction Diagnostics，端到端跨组件事务诊断功能](https://docs.microsoft.com/zh-cn/azure/application-insights/app-insights-transaction-diagnostics)。但是目前的监测工具体系中缺乏一个标准化的平台来实现度量和分布式跟踪, 这些数据需要支持跨编程语言和技术栈。微软宣称将利用自己的经验和知识与 OpenCensus 社区合作, 在应用程序度量和分布式跟踪领域创建一个开放和可扩展的标准平台, 从而使所有客户受益。

>Our goal is to leverage our experience and knowledge and combine it with that of OpenCensus community to create an open and extensible, standard platform for application metrics and distributed traces that will benefit all customers.  

- [Application Map，应用程序映射|会审分布式应用程序](https://docs.microsoft.com/zh-cn/azure/application-insights/app-insights-app-map)

应用程序映射可帮助发现性能瓶颈或热点失败的所有组件的分布式应用程序。在地图上的每个节点表示应用程序组件或其依赖项;并且有运行状况 KPI 和警报状态。可从任何组件单击以获得更详细的诊断，如 Application Insights 事件。 如果应用使用了 Azure 服务，还可以单击获得 Azure 诊断，如 SQL 数据库顾问建议。

![](http://riboseyim-qiniu.riboseyim.com/MS-ApplicationMap.png)

- [End-to-End Transaction Diagnostics，端到端跨组件事务诊断](https://docs.microsoft.com/zh-cn/azure/application-insights/app-insights-transaction-diagnostics)

事务诊断功能将所有受 Application Insights 监视的组件中的服务器端遥测关联到一个单独的视图。Application Insights 可检测基础关系，并可用于诊断导致事务缓慢或失败的应用程序组件、依赖项或异常。

![](http://riboseyim-qiniu.riboseyim.com/MS-PartscrossComponent.png)



## Tips

- [OpenCensus People](https://github.com/orgs/census-instrumentation/people)
- [OpenZipkin QuickStart](https://zipkin.io/pages/quickstart.html)

#### Docker

```bash
$ docker image pull openzipkin/zipkin
$ docker run -d -p 9411:9411 openzipkin/zipkin
$ docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                              NAMES
65d6aa99ea41        openzipkin/zipkin   "/bin/bash -c 'test …"   57 seconds ago      Up 56 seconds       9410/tcp, 0.0.0.0:9411->9411/tcp   zealous_shockley

```

#### Source

```bash
# get the latest source
git clone https://github.com/openzipkin/zipkin
cd zipkin
# Build the server and also make its dependencies
./mvnw -DskipTests --also-make -pl zipkin-server clean install
# Run the server
java -jar ./zipkin-server/target/zipkin-server-*exec.jar
```

```c
********
**        **
*            *
**            **
**            **
**          **
**        **
********
	****
	****
****                          ****
******                           ****                                 ***
****************************************************************************
*******                           ****                                 ***
****                          ****
	 **
	 **


*****      **     *****     ** **       **     **   **
**       **     **  *     ***         **     **** **
**        **     *****     ****        **     **  ***
******     **     **        **  **      **     **   **

:: Powered by Spring Boot ::         (v2.0.1.RELEASE)

```

## 扩展阅读

#### 分布式追踪系统
- [DevOps 漫谈：开源分布式跟踪系统 OpenCensus](https://riboseyim.github.io/2018/04/27/DevOps-OpenCensus)
- [DevOps 漫谈：分布式追踪系统标准体系](https://riboseyim.github.io/2018/05/18/DevOps-OpenTracing/)
- [远程通信协议：从 CORBA 到 gRPC](https://riboseyim.github.io/2017/10/30/Protocol-gRPC/)
- [应用程序开发中的日志管理(Go语言描述)](https://riboseyim.github.io/2017/05/24/Log/)

#### 动态追踪技术
- [动态追踪技术(一)：DTrace 导论](https://riboseyim.github.io/2016/11/26/DTrace/)
- [动态追踪技术(二)：strace+gdb 溯源 Nginx 内存溢出异常 ](https://mp.weixin.qq.com/s?__biz=MjM5MTY1MjQ3Nw==&mid=2651939588&idx=1&sn=35f71c5f88d1edf23cb2efc812ab8e6c&chksm=bd578c168a20050041c08618281691f0111f61c789097a69095933057618637fc54817815921#rd)
- [动态追踪技术(三)：Tracing Your Kernel Function!](https://riboseyim.github.io/2017/04/17/DTrace_FTrace/)
- [动态追踪技术(四)：基于 Linux bcc/BPF 实现 Go 程序动态追踪](https://riboseyim.github.io/2017/06/27/DTrace_bcc/)
- [动态追踪技术(五)：Welcome DTrace for Linux](https://riboseyim.github.io/2018/02/16/DTrace-Linux/)

#### 开源架构技术漫谈
- [基于Go语言快速构建一个RESTful API服务](https://riboseyim.github.io/2017/05/23/RestfulAPI/)
- [基于Kafka构建事件溯源型微服务](https://riboseyim.github.io/2017/06/12/OpenSource-Kafka-Microservice/)
- [数据可视化（七）Graphite 体系结构详解](https://riboseyim.github.io/2017/12/04/Visualization-Graphite/)
- [DevOps 资讯 | LinkedIn 开源 Kafka Monitor](https://riboseyim.github.io/2016/08/15/OpenSource-Kafka/)

## 参考文献
- [“Hello, world!” for web servers in Go with OpenCensus](https://medium.com/@orijtech/hello-world-for-web-servers-in-go-with-opencensus-29955b3f02c6)
- [OpenCensus: A Stats Collection and Distributed Tracing Framework | Wednesday, January 17, 2018 | Google Open Source Blog](https://opensource.googleblog.com/2018/01/opencensus.html)
- [OpenCensus for Go gRPC developers](https://medium.com/@orijtech/opencensus-for-go-grpc-developers-7f3ee1ac3d6d)
- [Uber正式开源其分布式跟踪系统Jaeger | 2017年11月9日](http://www.infoq.com/cn/news/2017/11/Uber-open-spurce-Jaeger)
- [Uber优步分布式追踪技术再度精进](http://www.infoq.com/cn/articles/evolving-distributed-tracing-at-uber-engineering)
- [Measure Once — Export Anywhere: OpenCensus in the wild](https://blog.doit-intl.com/measure-once-export-anywhere-opencensus-in-the-wild-61724f44eb00)
- [opencensus-go-examples-helloworld-context](https://github.com/census-instrumentation/opencensus-go/blob/master/examples/helloworld/main.go)
- [MongoDB driver instrumented with OpenCensus in Go](https://medium.com/@orijtech/mongodb-driver-instrumented-with-opencensus-in-go-e691370b8184)
- [Go语言并发模型：使用 context | oscarzhao 2016年08月29日](https://segmentfault.com/a/1190000006744213#articleHeader8)
- [理解Go Context机制 | 时间： 2016-08-02](http://lanlingzi.cn/post/technical/2016/0802_go_context/)
- [Go语言实战笔记（二十）| Go Context | May 12, 2017](http://www.flysnow.org/2017/05/12/go-in-action-go-context.html)
