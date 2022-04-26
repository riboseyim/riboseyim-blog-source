---
title: 数据可视化（五）基于网络爬虫制作可视化图表
date: 2017-05-12 18:45:50
tags: [数据可视化,Developer,Golang]
categories: 工程技术
---
## 摘要
- 基于网络爬虫的可视化图表:golang,goquery
- 案例：最近十年全国彩票销售变化情况
- 案例：中国科学院院士分布
- 数据可视化技术方案:基于 SVG (D3、Raphael)、基于 Canvas（Echarts）

<!--more-->

## 基于网络爬虫的可视化图表

我们身处大数据时代，几乎在所有工作例如商业技术、金融、科研教育等行业，以及日常生活中都可能需要涉及数据分析活动。横向来看数据分析的知识体系贯穿数据获取、数据存储、数据分析、数据挖掘、数据可视化等各大部分；按数据来源分，即可以是自己收集的数据，也可以采购数据或者基于公开数据集。

基于公开数据进行分析的话，必须提到的就是网络爬虫（web crawler），也被称作网络蜘蛛（spider）、自动索引程序（automatic indexer），搜索引擎（Google，百度等）就是大众日常生活中接触到的最典型、最强大的爬虫。

公开数据包括政府（统计局、央行、银监会、证监会等）、企业、社会组织和互联网上的个人发布信息等。在浩如烟海的互联网内容中，有价值信息犹如‘待字闺中’深藏的美女，等待有心人去挖掘。例如：

- 案例：最近十年全国彩票销售变化情况 [在线演示](https://riboseyim.github.io/charts/caipiao/index.html)
- 案例：中国科学院院士分布（出生地与籍贯）[在线演示](https://riboseyim.github.io/charts/casad/index.html)
- 案例：美国航空入境旅客（出发地）变化情况 [在线演示](https://riboseyim.github.io/charts/usa-traffic/index.html)

![中科院院士分布情况|201801](http://riboseyim-qiniu.riboseyim.com/%E4%B8%AD%E5%9B%BD%E7%A7%91%E5%AD%A6%E9%99%A2%C2%B7%E9%99%A2%E5%A3%AB%E5%88%86%E5%B8%83%E6%83%85%E5%86%B5%C2%B7201801.png)

![全国彩票销售情况](http://riboseyim-qiniu.riboseyim.com/gmof-caipiao-3.png)

![](http://riboseyim-qiniu.riboseyim.com/usa-traffic-1980.png)

为了实现上述图表，相关技术方案的要点如下：
- 开发语言：
基于 [Golang](https://riboseyim.github.io/2017/05/05/Language-Go-lang/) 实现爬虫基本功能，主要考虑 [Go 语言](https://riboseyim.github.io/2017/05/05/Language-Go-lang/) 自身对于网络方面的强大支持，语言级 Goroutines 提供并发高性能支持。
- HTML选择器: **goquery** jQuery-style HTML manipulation in Go
- 数据存储: csv,[PostgreSQL](https://riboseyim.github.io/2018/01/03/OpenSource-DB-PostgreSQL/) 等
- 数据可视化：**ECharts**

![基于网络爬虫制作可视化图表](http://riboseyim-qiniu.riboseyim.com/%E5%A4%A9%E5%AE%98%E8%B5%90%E7%A6%8F.png)

#### 案例

数据来源页面：

- [专栏：彩票管理](http://zhs.mof.gov.cn/zhuantilanmu/caipiaoguanli/)
- [2017年11月份全国彩票销售情况](http://zhs.mof.gov.cn/zhuantilanmu/caipiaoguanli/201712/t20171221_2786493.html)
- [全体院士名单](http://www.casad.cas.cn/chnl/371/index.html)
- [院士个人介绍](http://www.casad.cas.cn/aca/371/sxwlxb-200906-t20090626_1854608.html)

![数据来源-专题](http://riboseyim-qiniu.riboseyim.com/gmof-caipiao-process-4-min.png)
![数据来源-内容](http://riboseyim-qiniu.riboseyim.com/gmof-caipiao-process-3.png)
![数据来源-翻页](http://riboseyim-qiniu.riboseyim.com/gmof-caipiao-process-5.png)

![数据来源页面-源代码](http://riboseyim-qiniu.riboseyim.com/gmof-caipiao-process-2.png)
![数据来源页面-源代码](http://riboseyim-qiniu.riboseyim.com/gmof-caipiao-process-1.png)

```go
//caipiao_task.go

func Handle_GMOF_CaiPiao_Month_BatchTask() {
	data := read_csv_caipiao("./data/caipiao_list.csv", ",")
	if data != nil {
		for i := range data {
			go Handle_GMOF_CaiPiao_Month_Task(url)
		}
		<-time.After(60 * time.Second)
	}
}

func Handle_GMOF_CaiPiao_Month_Task(url string) {
	if url != "" {
		myspider := init_GMOF_CaiPiao_Month_HTMLSpider(url)
		ctx, _ := myspider.Setup(nil)
		myspider.Spin(ctx)
	}
}

```

```go
//caipiao_spider.go
package main

import (
	"log"
	"regexp"
	"strings"

	"github.com/PuerkitoBio/goquery"
	"github.com/celrenheit/spider"
)

type GMOF_CaiPiao_Month_HTMLSpider struct {
	title string `json:"title"`
	url   string `json:"url"`
	desc  string `json:"desc"`
}

func init_GMOF_CaiPiao_Month_HTMLSpider(url string) *GMOF_CaiPiao_Month_HTMLSpider {
	spider := NewGMOF_CaiPiao_Month_HTMLSpider()
	spider.url = url
	return spider
}

func (w *GMOF_CaiPiao_Month_HTMLSpider) Setup(ctx *spider.Context) (*spider.Context, error) {
	return spider.NewHTTPContext("GET", w.url, nil)
}

func (w *GMOF_CaiPiao_Month_HTMLSpider) Spin(ctx *spider.Context) error {
	if _, err := ctx.DoRequest(); err != nil {
		return err
	}

	html, err := ctx.HTMLParser()
	if err != nil {
		return err
	}

	caipiao := NewGMOF_CaiPiao_Month()

	//<title></title>
	caipiao.Title = html.Find("title").Eq(0).Text()
	caipiao.Title = Convert2String(caipiao.Title, GB18030)
	//class="TRS_Editor"
	html.Find(".TRS_Editor").Each(func(i int, s *goquery.Selection) {
		content := s.Find("p").Text()
		caipiao.Content = content

		if content != "" {
			content = Convert2String(content, GB18030)
			rows := strings.Split(content, "。")

			for _, value := range rows {
				//fmt.Printf("======arr[%d]=\n [%s] \n", index, value)
				if strings.Index(value, "全国彩票") > 0 {
					reg := regexp.MustCompile(`全国共销售彩票([\d]+.[\d]+)\S+`)
					result := reg.FindStringSubmatch(value)
					if len(result) > 0 {
						caipiao.Total = result[1]
					}
				}
			}
		}
	})

	//id="appendix"
	html.Find("#appendix").Each(func(i int, s *goquery.Selection) {
		href, _ := s.Find("a").Eq(0).Attr("href") //附件
		caipiao.Attachid = href
	})

	//===== export data
	save_csv("./data/caipiao_result.csv", caipiao)
	return err
}
```

>2017年11月份全国彩票销售情况,385.55
2017年10月份全国彩票销售情况,376.53
2017年9月份全国彩票销售情况,369.28
2017年8月份全国彩票销售情况,350.67
2017年7月份全国彩票销售情况,337.55
2017年6月份全国彩票销售情况,338.42

#### 可视化图表：以 ECharts 为例

**常见的图表库**，本文案例使用 ECharts 作为图表组件
- [HighCharts](http://www.highcharts.com)：JavaScript 编写，开源许可证允许个人用户和非商业用途。
- [Baidu ECharts](http://echarts.baidu.com)：底层画图基于 Canvas, BSD 许可证协议。
- [Kartograph](http://www.oschina.net/p/kartograph?fromerr=tZ15BKGv)：构建交互式地图轻量级类库。

```JavaScript
//http://echarts.baidu.com/demo.html#line-gradient
data = [["2017年11月",385.55],["2017年10月",376.53],["2017年9月",369.28],["2017年8月",350.67],["2017年7月",337.55],["2017年6月",338.42],["2017年11月",385.55],["2017年10月",376.53],["2017年9月",369.28],["2017年8月",350.67],["2017年7月",337.55],["2017年6月",338.42],["2017年11月",385.55],["2017年10月",376.53],["2017年9月",369.28],["2017年8月",350.67],["2017年7月",337.55],["2017年6月",338.42],["2017年5月",376.95],["2017年4月",382.45],["2017年3月",379.33],["2017年2月",0],["2017年1月",291.61],["2016年12月",365.94],["2016年11月",344.82],["2016年10月",338.27],["2016年9月",320.71],["2016年8月",310.12],["2016年7月",324.03],["2016年6月",339.61],["2016年5月",346.19],["2016年4月",348.89],["2016年3月",356.88],["2016年2月",224.54],["2016年1月",326.41],["2015年12月",341.21],["2015年11月",306.30],["2015年10月",312.34],["2015年9月",290.78],["2015年8月",280.96],["2015年7月",270.47],["2015年6月",281.2371],["2015年5月",321.07],["2015年5月",321.07],["2015年4月",326.12],["2015年3月",308.12],["2015年2月",247.90],["2015年1月",392.33],["2014年12月",361.53],["2014年11月",341.18],["2014年10月",327.01],["2014年9月",322.52],["2014年8月",315.36],["2014年7月",372.09],["2014年6月",360.54],["2014年5月",307.94],["2014年4月",315.29],["2014年3月",328.74],["2014年2月",200.1],["2014年1月",271.49],["2013年12月",302.73],["2013年11月",274.16],["2013年10月",271.83],["2013年9月",257.62],["2013年8月",246.18],["2013年7月",243.65],["2013年6月",247.46],["2013年5月",273.41],["2013年4月",285.61],["2013年3月",273.37],["2013年2月",168.65],["2013年1月",248.59],["2012年12月",268.01],["2012年11月",237.06],["2012年10月",215.38],["2012年9月",205.12],["2012年8月",197.12],["2012年7月",201.98],["2012年6月",216.14],["2012年5月",236.16],["2012年4月",235.76],["2012年3月",235.79],["2012年2月",202.17],["2012年1月",164.54],["2011年12月",224.80],["2011年11月",210.08],["2011年10月",203.28],["2011年9月",196.44],["2011年8月",187.72],["2011年7月",182.05],["2011年6月",174.53],["2011年5月",187.28],["2011年3月",190.12],["2011年2月",112.92],["2011年1月",160.09],["2010年12月",171.89],["2010年11月",160.24],["2010年10月",149.95],["2010年9月",139.56],["2011年4月",186.50],["2010年8月",135.75],["2010年7月",132.74],["2010年6月",140.71],["2010年5月",144.38],["2010年4月",141.05],["2010年3月",132.52],["2010年2月",86.71],["2010年1月",126.99],["2009年12月",133.30],["2009年11月",117.05],["2009年10月",116.47],["2009年9月",111.73],["2009年8月",110.64],["2009年7月",107.87],["2009年6月",113.51],["2009年5月",121.59],["2009年4月",114.61],["2009年3月",114.49],["2009年2月",89.21],["2009年1月",74.33],["2008年12月",102.07],["2008年11月",94.09],["2008年10月",79.88],["2008年8月",84.66]];
var dateList = data.map(function (item) {
    return item[0];
});
var valueList = data.map(function (item) {
    return item[1];
});

option = {
    // Make gradient line here
    visualMap: [{
        show: false,
        type: 'continuous',
        seriesIndex: 0,
        min: 0,
        max: 400
    }, {
        show: false,
        type: 'continuous',
        seriesIndex: 1,
        dimension: 0,
        min: 0,
        max: dateList.length - 1
    }],
    title: [{
        left: 'center',
        text: 'Gradient along the y axis'
    }, {
        top: '55%',
        left: 'center',
        text: 'Gradient along the x axis'
    }],
    tooltip: {
        trigger: 'axis'
    },
    xAxis: [{
        data: dateList
    }, {
        data: dateList,
        gridIndex: 1
    }],
    yAxis: [{
        splitLine: {show: false}
    }, {
        splitLine: {show: false},
        gridIndex: 1
    }],
    grid: [{
        bottom: '60%'
    }, {
        top: '60%'
    }],
    series: [{
        type: 'line',
        showSymbol: false,
        data: valueList
    }, {
        type: 'line',
        showSymbol: false,
        data: valueList,
        xAxisIndex: 1,
        yAxisIndex: 1
    }]
};
```

#### 最佳实践

- 默认调色板(palette)

![](http://riboseyim-qiniu.riboseyim.com/Coror-Default.png)

```js
Navy    — #001f3f
Blue    — #0074d9
Aqua    — #7fdbff
Teal    — #39cccc
Olive   — #3d9970
Green   — #2ecc40
Lime    — #01ff70
Yellow  — #ffdc00
Orange  — #ff851b
Red     — #ff4136
Maroon  — #85144b
Fuchsia — #f012be
Purple  — #b10dc9
Black   — #111111
Gray    — #aaaaaa
Silver  — #dddddd
White   — #ffffff
```

- 优化图表JS生成模板
图表定型之后，可以通过模板固化配置，根据需要动态生成目标文件（html,js,svg等等），详见[基于 Markdown 的 HTML 网页模板](https://riboseyim.github.io/2017/12/21/Language-Auto-Generator/)。

- 优化采集器 Goroutines "线程池"
例如：PostgreSQL Exception: Open too many files

- 优化数据存储
例如：常用的 GIS 坐标库

## 扩展阅读：开源工具与案例

#### 在线词云
- [WordArt.com](http://www.wordart.com):仅支持英文，精美，首选
- [Tagxedo](http://www.tagxedo.com/app.html):支持中、英文（中文分词效果一般），需要安装插件Silverlight
- [Tag Crowd & Google Adwords](https://mp.weixin.qq.com/s/USIakoaavuy7XkO-5C0tgA)

#### golang-based library
- [golang.org/net/http](https://golang.org/pkg/net/http/)
- [github.com/celrenheit/spider](https://github.com/celrenheit/spider)
- [goquery: jQuery-style HTML manipulation in Go](https://www.progville.com/go/goquery-jquery-html-golang/)
- [github.com/henrylee2cn/pholcus_lib](https://github.com/henrylee2cn/pholcus_lib)
- [Pholcus is a distributed, high concurrency and powerful web crawler software](https://github.com/henrylee2cn/pholcus)

#### 可视化图表案例
- [中国主要城市空气质量实况](http://echarts.baidu.com/doc/example/topic/aqi-china/index.html)
- [中国经济十年时空漫游（2002-2011）](http://echarts.baidu.com/doc/example/topic/10-me-china/index.html)

## 可视化图表技术方案
- 基于 SVG : D3、Raphael
- 基于 Canvas : Echarts

- [HighCharts](http://www.highcharts.com)
国外开源产品，JavaScript 编写，自带主题、动态交互方便，目前公司新版业务视图、地图应用、交互式流量图等是基于这个库实现。
不足：缺少中文文档，开源许可证只允许个人用户和非商业用途，规模应用存在法律风险。

- [Baidu ECharts](http://echarts.baidu.com)
最早源于百度各种业务系统报表需求，底层画图基于 Canvas 。2013年开源，完全免费的BSD协议。
特点：拖拽重计算，第三方标准格式支持，中文社区支持
实例：http://echarts.baidu.com/doc/example.html
Github: https://github.com/ecomfe/echarts  
![](http://riboseyim-qiniu.riboseyim.com/echarts-%E5%9B%BD%E5%AE%B6%E7%BB%9F%E8%AE%A1%E5%B1%80.png)

- [Kartograph](http://www.oschina.net/p/kartograph?fromerr=tZ15BKGv)
Kartograph 是个构建交互式地图的简单、轻量级类库。它包含两个库，一个用Python写的，用于产生漂亮和压缩的SVG地图，另一个是js类库用于前端展示地图用。

- [lchart(go-based)](https://github.com/ajstarks/deck/tree/master/cmd/lchart)
- [朝花夕拾|showtext：字体，好玩的字体和好玩的图形 | 原创 2016-11-06 统计之都 统计之都](https://mp.weixin.qq.com/s/FXeDOHg_k52nv4We08SeQQ)

- [Themes for ggplot2.](https://github.com/cttobin/ggthemr)

## 扩展阅读：数据可视化
- [数据可视化（一）思维利器 OmniGraffle 绘图指南 ](https://riboseyim.com/2017/09/15/Visualization-OmniGraffle/)
- [数据可视化（二）跑步应用Nike+ Running 和 Garmin Mobile 评测](https://riboseyim.com/2016/04/26/Visualization-BestAppMap)
- [数据可视化（三）基于 Graphviz 实现程序化绘图](https://riboseyim.com/2017/09/15/Visualization-Graphviz/)
- [数据可视化（四）开源地理信息技术简史（Geographic Information System](https://riboseyim.com/2017/05/12/Visualization-GIS/)
- [数据可视化（五）基于网络爬虫制作可视化图表](https://riboseyim.com/2017/05/12/Visualization-Charts/)
- [数据可视化（六）常见的数据可视化仪表盘(DashBoard)](https://riboseyim.com/2017/11/23/Visualization-DashBoard/)
- [数据可视化（七）Graphite 体系结构详解](https://riboseyim.com/2017/12/04/Visualization-Graphite/)
- [数据可视化（八）Program,Data and Classical Music](https://riboseyim.com/2018/12/16/Visualization-SocialNetwork/)
- [数据可视化（十）公共数据源列表](https://riboseyim.com/2018/01/15/Visualization-DataSource/)

## 参考文献
- [数据从业者必读：抓取了一千亿个网页后我才明白，爬虫一点都不简单 | 算法与数学之美  2018-09-07](https://mp.weixin.qq.com/s/t668acG5lRZxZ7A0xMXTSg)
- [Matplotlib 可视化最有价值的 14 个图表（附完整 Python 源代码） | Lemonbit  GitChat精品课  3月13日](https://mp.weixin.qq.com/s/dw-c-nV8TJK5WWzSPal-yA)
