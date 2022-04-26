---
title: 数据可视化（一）思维利器 OmniGraffle for Mac 使用指南
date: 2017-09-15 16:31:40
tags: [数据可视化,工具癖,Mac,Engineering]
categories: 工程技术
---
## 摘要
OmniGraffle 是由 The Omni Group 制作的一款绘图软件，它曾获得苹果设计奖。OmniGraffle 可以支持流程图、逻辑图或者网页产品模型设计等，功能非常强大。与 Graffle 对应的是在Windows平台广泛应用的 MS Visio（ Graffle 这个词据说就是为了和Visio区分而硬造出来的），关于这两个产品的用户体验对比，本文会稍有涉及。

关于它的使用细节——术的方面，建议读者直接参考帮助文档或平台上其他作者的教程。本文 **重点** 想探讨的，是在工程实践中的一些方法论——跟“道”有关的一些个人体会。

[首发版本：《最佳工程时间——思维利器 OmniGraffle》| 简书-201601 ](http://www.jianshu.com/p/ccc8d64c7202)，本文有更新修正。

<!--more-->
## 一、思维可视化

一般人的大部分思考过程都是杂乱无序的，没有逻辑的，最后也没法形成有效的沉淀，更无法找到清晰的结论。不是所有的人都是天生就有很好的逻辑的，但是逻辑是可以训练的，只要你懂的把自己的思维进行可视化的展示、分析和整理。

- 常见的思维可视化模型

1. 放射状规整（思维导图、鱼骨图）－－以后专门讨论（例如 MindManager 等）
2. 层次化规则（架构图、组织结构图）
3. 线性化规整（路径图、时间线）
4. 矩阵式规则（SWOT 分析、商业模式画布）

## 二、数据流

概要数据流可以快速梳理出数据流向，顺手还能体现数据模型的结构关系。
![数据流示例](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_5.png)

## 三、业务流

#### 第一步：确定边界
开始可能不太明了，或者中途有新的变更，但是不要紧，只要搞清楚起点和终点，确定边界&关键里程碑，解决从哪里来、到哪里去的终极问题，就能纲举目张画出全局流程图，之后再集中精力各个击破。

![概要总图](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_4.png)

#### 第二步：针对不同子流程各个击破

![子流程1](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_12.png)

![子流程2](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_14.png)

#### 第三步：不仅仅是几个框
完成第二步之后，传统语境下的流程图就完成了。但是，作为一个有追求的演员，似乎还可以尝试点什么。突然，一道灵光撞击出来新的想法。事不宜迟，马上记下来。拖几个元素，插入表格、贴上截图，简单的概要原型页就出来了。神来之笔：借用IOS风格的搜索栏，网页原型是不是颇有大写意风格。

![概要原型图](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_7.png)

#### 第四步：文档归集

在工程实践中，项目或产品文档的管理从来都是一个难题。从初识阶段开始，产品、设计、开发、测试、运营团队在组织架构上可能是分离的，大家本能地按自己的风格习惯写文档，即使是最重要的几个文档，也常常是Visio、PowerDesign、Word、PPT等各种格式混杂，每种文档可能还有软件版本的兼容性差异。再加上积年累月的人员更迭、团队重组，各种混乱交接，有时查一个简单的问题，都可能要打开电脑上所有的办公软件、十几个窗口。这是要疯掉的节奏，非常非常低效！基于OmniGraffle良好的兼容特性，完全可以将关键文档整合到一个Graffle文件，不但可以为当前进行的工作中保持迭代、保护成果，还能持续收拢、归纳、索引关键信息，为后续的改进优化打下好的基础。

- 文档树
- 开局索引
- 添加动作
- 跳转动作特性：切换版面、显示／隐藏图层、运行脚本
- 多类型分发：支持导出Pdf/PNG/HTML/Graffle模版等
- 导出文件动作保真：Pdf文件支持亦支持动作跳转

![开局索引](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_6.png)

![文档树](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_8.png)

![跳转动作特性](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_9.png)

## 四、技巧

#### 1、构建组件库

Graffle和Visio一样，提供了扩展资源库的支持，想要什么组件，搜一搜就有了。然而很多时候，我们面临问题不是缺少资源，而是资源太多，无法快速作出选择。正如制造工厂里的模具，码农的代码库，有逼格的人一定受不了拖个按钮都得搜索、调色半天。Graffle自带的资源、以前设计的组件、搜罗来的图片/PPT/Visio资源，日常工作中新发明的兵器，最好分门别类放到一个专门的版面。从最原始各种大小字体、点、线条、箭头开始，逐渐积累起组件，甚至样板页，不断吸收、调整、优化，愈积愈厚，直至形成自己的风格，是不是有点像逍遥派的北冥神功？当然，不排除牛人们是从一开始就可以开发出好几套不同Style的套装。

- 检索扩展资源库
- Build Style Library

![检索扩展资源库](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_11.png)

![Style Library](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_1.png)

#### 2、技术资产保护

对于众多的用户来说，最大限度的复用现有数据成果，对于技术资产保护具有重要意义。包括但不限于不同工具的成果复用和转换，以及能够将存量数据迁移到 OminGraffle 非常重要。原来是 Windows 平台的用户有一个非常友好的特性，即可以在 OmniGraffle 直接打开MS Visio文件,并在此基础上继续编辑。

- 从MS Visio迁移
- 从Axure迁移
- 其它迁移：截图 & 外部图片

![从MS Visio迁移](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_2.png)
![从Axure迁移](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_13.png)

#### 3、踩过的坑

- 自动布局非常坑爹
- 自动布局的适用场景

OmniGraffle新建文件的初始设置默认是自动布局的。每当你增加一个元素，调整一下位置，command+S , 我去！！刚才的努力白费了，它会按照一套匪夷所思的对齐规则重新布局！ 除了非常简单的图形，是在想不出实际工作中有什么东西是按照经典原型排列的。好吧，我承认，因为这个坑爹的原因。我大概几个月的时间不知道怎么用这玩意，差点放弃！！虽然开始在自动布局上踩了坑，但是这招在某些情况下还是非常有用的，例如下面这个，不是美术系的怎么可能画这么圆?  还是那句话，特性很多，都有适用范围，合适就好。

![自动布局-操作按钮](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_3.png)

![自动布局-环形效果图](http://riboseyim-qiniu.riboseyim.com/OmniGraffle_Action_201601_10.png)

## 拓展阅读：工作效率软件
- [最佳写作实践：从Evernote到Ulysses](http://www.jianshu.com/p/7c453ce42150)
- [高效创作：我的写作工具链](https://riboseyim.com/2017/06/03/Writing-WriterToolChain/)
- [大道至简：我的项目管理工具集](https://riboseyim.com/2016/04/26/TeamWork-Redmine/)

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
- [精品幻灯片 | What’s New in Apple Filesystems](https://devstreaming-cdn.apple.com/videos/wwdc/2019/710aunvynji5emrl/710/710_whats_new_in_apple_file_systems.pdf)
- [如何画好架构图](https://mp.weixin.qq.com/s/MNXeu7d1P4dYBZVr1Zhy5A)
