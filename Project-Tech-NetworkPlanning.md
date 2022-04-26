---
title: PM指南:网络计划技术|工具与技术
date: 2019-05-29 14:05:39
tags: [工具癖,Engineering]
categories: 工程技术
---
## 摘要

<!--more-->

## Background

#### Question

#### Theory

#### History

网络计划技术是一种用于工程项目计划与控制的管理技术。起源于五十年代末发展起来的关键路径法（CPM）与计划评审法（PERT）。鉴于技术起源和设计目的，业内一般将 CPM 应用于已取得一定经验的承包工程，而研究与开发项目则更多地应用 PERT 。


| 工具        | 起源                                | 时间  | 成本   | 质量   | 费用   |
| ----------- | ----------------------------------- | ------ | ------ | ------ | ------ |
| 甘特图      |                                     |        |        |        |        |
| 关键路径法/CPM | 1957，美国杜邦公司/兰德公司         | weak   | strong | weak   | strong |
| 技术评审技术/PERT     | 1958，美国海军/洛克希德，潜射核导弹 | strong | weak   | strong | weak   |

##### 甘特图（Gantt Chart）

发明人：亨利 ·甘特(Henry Gatt)

##### 关键路径法(Critical Path Method,CPM)

关键路径法（CPM）是由美国杜邦公司和兰德公司与1957年联合研究提出的，它假设每项活动的作业时间是确定值，重点在于费用和成本的控制。

![](http://riboseyim-qiniu.riboseyim.com/OmniPlan.png)

##### 计划评审技术(Project Evaluation and Review Technique,PERT)

项目评估与审查技术（Project Evaluation and Review Technique,PERT）由美国海军首次提出，并于1958年由 Booz、Allen 和 Hamilton 的咨询公司开发。最早的用途是协调涉及北极星导弹研发计划的 10 000 多个分包商的活动。PERT 类似关键路径方法(CPM)，是一种用于优化和调度复杂的、相关关联的活动的方法。


## Introduction | 网络计划技术简介

网络计划技术（Network Planning Technology）

![](http://riboseyim-qiniu.riboseyim.com/Tool-%E7%BD%91%E7%BB%9C%E8%A7%84%E5%88%92%E6%8A%80%E6%9C%AF.png)

## Core Concept | 核心概念  

#### Concept 网络图

网络图是指网络计划技术的图解模型，反映整个工程任务的分解和合成。

#### Concept 时间参数

各项工作中反映人、事、物运动状态的时间参数包括：
- 作业时间
- 开工与完工的时间
- 工作之间的衔接时间
- 完成任务的机动时间
- 工程范围
- 总工期

#### Concept 关键路线

在关键路线上的作业称为关键作业，这些作业完成的快慢直接影响着整个计划的工期。在计划执行过程中关键作业是管理的重点，在时间和费用方面则要严格控制。

#### Concept 网络优化

网络优化是指根据关键路线法，通过利用时差，不断改善网络计划的初始方案，在满足一定的约束条件下，寻求管理目标达到最优化的计划方案。

#### 实施步骤

网络计划技术的应用主要遵循以下几个步骤：

- 1、确定目标；
- 2、分解工程项目，列出作业明细表；
- 3、绘制网络图，进行结点编号；主要方法：1）顺推法（起始时间开始）；2）逆推法（从终点时间开始）
- 4、计算网络时间，确定关键路线；根据网络图和各项活动的作业时间计算出全部网络时间和时差，并确定关键线路。在实际工作中，作业种类多、影响计划的因素也很多，因此通常需要依靠计算机对计划进行调整。
- 5、网络计划方案优化；根据关键路径，综合平衡总工期、人力资源、物资供应、成本费用等情况，制定最优方案。正式绘制网络图，编制进度表以及工程预算等计划文件。
- 6、网络计划执行

| 作业名称 | 作业代号 | 作业时间 | 紧前时间 | 紧后时间 |
| -------- | -------- | -------- | -------- | -------- |
|          |          |          |          |          |

## Best Practice | 最佳实践

#### Microsoft Office Project

#### OmniGroup OmniPlan


## Future：Beyond the Network

## 阅读作业

## 拓展阅读

#### 项目管理

- [项目管理 | Overview of Project Management](https://riboseyim.com/2019/02/06/Project/)
- [PM指南:PMI项目管理知识体系](https://riboseyim.com/2019/04/30/Project-PMP/)
- [PM指南:建筑工程项目管理|行业案例教学](https://riboseyim.com/2019/03/27/Project-Construction/)
- [PM指南:范围管理](#)
- [PM指南:进度管理](#)
- [PM指南:成本管理](#)
- [PM指南:质量管理](#)
- [PM指南:资源管理](https://riboseyim.github.io/2019/03/12/Project-Resources/)
- [PM指南:沟通管理](https://riboseyim.com/2019/02/06/Project-Communications/)
- [PM指南:风险管理](https://riboseyim.github.io/2018/06/05/Project-Risk/)
- [PM指南:采购管理](https://riboseyim.github.io/2019/03/12/Project-Procurement/)
- [PM指南:相关方管理](#)
- [PM指南:网络计划技术|工具与技术](https://riboseyim.com/2019/05/29/Project-Tech-NetworkPlanning/)
- [PM指南:项目管理信息系统|工具与技术](https://riboseyim.com/2019/04/06/Project-PMIS/)
- [PM指南:项目管理开局模板|工具与技术](https://riboseyim.com/2018/06/19/Project-Template/)
- [PM指南:开源项目管理平台Redmine|工具与技术](https://riboseyim.com/2016/04/26/TeamWork-Redmine/)
- [PM指南:软件业看板Kanban管理实践|工具与技术](https://riboseyim.com/2017/08/06/TeamWork-Kanban/)
- [PM指南:源代码版本管理|工具与技术](https://riboseyim.com/2016/05/31/TeamWork-Git/)


## 参考文献
- [北极星导弹项目主页](https://web.archive.org/web/20071019072438/http://www.lockheedmartin.com/products/Polaris/index.html)
- [如何有效使用Project（1）——编制进度计划、保存基准](https://www.cnblogs.com/wangfupeng1988/p/3647166.html)
