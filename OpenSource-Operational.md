---
title: 开源软件的运营挑战
date: 2017-11-05 22:12:41
tags: [OpenSource,DevOps,Linux,Engineering]
categories: 工程技术
---
## 摘要

<!--more-->

## The Big Picture

- 原文：[6 Operational Challenges to Using Open Source Software | By Linux FoundationMarch 15, 2017Blog](https://www.linux.com/blog/learn/chapter/open-source-management/2017/3/6-operational-challenges-using-open-source-software)

>在当今迅速发展的市场中，那些速度最快、成本最低的持续创新公司才会赢 。同时，正如你所知，我们正在进行的一系列观察，使用开源软件能够实现快速、低成本的创新。 但它也能引入运营挑战和法律风险。

我们现在已经到了这样一个临界点，开源软件已经成为主流，不使用开源几乎肯定会使你的组织处于劣势。所以你必须学会如何驾驭挑战和风险，才能保持竞争力。

>“开源是无处不在的，它是不可避免的……对抗开源的政策是不切实际的，会使你处于竞争劣势。”——加特纳。

在这篇文章中，我们将探讨开源如何成为构建软件现实途径。然后，我们将介绍这种新的软件开发方法为组织引入的挑战。

#### 一、开源软件革命

从大约四十年前开始，发端于创新学术研究和GNU工具项目，开源软件已经成为主流并重塑了许多行业。今天，已经有超过150万个独特的开源项目，贡献给软件开发者的工作代码超过百万兆字节。这些资源的可用性、软件模块化的趋势以及软件重用，从根本上改变了大多数公司开发软件的方式。

不久以前，我们大部分软件产品都是自己内部开发的。我们也许会使用了一些第三方组件，用以连接到其他系统或执行一些特殊的业务，但这些都是通过一个精心控制的采购流程控制的。

今天，我们可以利用互联网上免费的开源组件，以更快的速度开发更复杂的软件。我们的大部分活动已经从设计开发定制软件，转变为整合现成的组件或半成品。我们只对应用程序中确实非常独特的部分进行编程。

但是现在，我们不再重复一些精心控制的代码收购，而是从互联网上反复下载代码来进行评估、原型制作和集成。虽然这种方法加快了开发速度，但它制造了一系列新的重大挑战。

#### 二、开源软件六大运营挑战

使用开源软件带来了许多好处，但它会在软件开发生命周期中引入风险和额外的操作复杂性。

1. 组织必须处理许多新的软件源代码，包括商业和非商业的供应商 – 一些人使用的开源软件来自数百个不同的来源。

2. 众多可用的开源组件导致需要大量的第三方软件采购决策。这些决策如何做出？许多开发者没有相应的资质考虑所有这些必要的方面，包括软件许可证分析，但是一个沉重复杂的流程，例如陈旧的采购方法，代价昂贵和而且费时很长，以至于无法应用到大量新的采购决策中。

3. 集成了大量的第三方组件会制造复杂性。其中一方面就是在多个相互依赖的代码栈中维护软件版本的一致性。

4. 开源软件项目的范围，覆盖业余玩家到专业的开发和版本测试等不同阶段。您的组织必须确保每个应用程序都能选择相应质量保障级别的组件。

5. 您的组织将如何获得所有这些开源组件的技术支持和更新？健康的开源社区提供优秀的支持和维护，但开源的自助服务模式，需要您的开发者与社区的共同参与这部分工作。

6. 商业合作关系可以通过增加财政奖励的方式，向供应商强化你的诉求主张，但是否能影响开源项目的方向取决于您参与的多方面因素。

## Application

### Microservice

- [project gizmo](https://github.com/nytimes/gizmo)

Gizmo 是一个集合，集中了可整合的工具、功能以及交互页面来帮助开发者构建 Go 服务，具体来说就是将 API 和 pubsub daemons 集合在一起。这个工具包刚开始的时候是由一小群开发者发起的，目的是为 NYT 构建一个专属邮件平台。

## 扩展阅读

- [基于Ganglia实现服务集群性能态势感知](https://riboseyim.github.io/2016/11/04/OpenSource-Ganglia/)

## 参考文献
- [6 Operational Challenges to Using Open Source Software | By Linux FoundationMarch 15, 2017Blog](https://www.linux.com/blog/learn/chapter/open-source-management/2017/3/6-operational-challenges-using-open-source-software)
- [纽约时报开发出基于 Go 语言的微服务工具包 Gizmo](https://www.infoq.cn/article/2016/02/gizmo-microservices-toolkit)
- [In Pursuit of Production Minimalism | 公司里面使用的技术要尽量少。不要引入太多种的编程语言、编程框架、不同类型的数据库等。定期淘汰过时的项目、顺便淘汰掉让人头疼的那些编程语言、框架、软件等。多引入一个新技术，就多了出事故的风险。](https://brandur.org/minimalism?utm_source=wanqu.co&utm_campaign=Wanqu+Daily&utm_medium=website)
