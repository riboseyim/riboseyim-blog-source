---
title: PM指南:开源项目管理平台Redmine
date: 2016-04-26 11:32:25
tags: [工具癖,Manager,Engineering]
categories: 工程技术
---
## 概要

>不管别人如何干扰，如何制造事端，最优秀的将领总是能达到预期的木笔而不会受制于作战计划。（温斯顿·S·丘吉尔《丘吉尔传·我的青春》p227）

<!--more-->

## Redmine

- [基于Redmine的项目管理平台](http://www.jianshu.com/p/cd7a12fa09bb)

"过程有记录，责任可追踪",开发、运营过程中需要持续关注的基础信息，都可以通过统一平台沉淀、分析和改进优化。

![](http://riboseyim-qiniu.riboseyim.com/gantt@2x.png)

![](http://riboseyim-qiniu.riboseyim.com/workflow_example@2x.png)

<!--![基于Redmine数据定制开发](http://riboseyim-qiniu.riboseyim.com/theme_redmine_myindex.png)-->

## BASIC

#### Install Redmine

[BitName](http://bitnami.com) 提供了一个包含 Ruby、RAILS、MySQL、Apache 等依赖的集成安装包。

```sh
# 1.解压
~/redmine-1.2.1-1/apps/redmine/vender/plugins
$ cd redmine-1.2.1-1
# 2.安装
$ ./use_redmine
# 3.配置
$ cd apps/redmine/vender/pluginsrake
$ db:migrate_plugins RAILS_ENV=production
```

#### 数据结构

![](https://upload-images.jianshu.io/upload_images/1037849-f81e70d040cb7850.jpg)


## Kanban

- [Kanban看板管理实践精要](https://riboseyim.com/2017/08/06/TeamWork-Kanban/)

![](http://riboseyim-qiniu.riboseyim.com/Kanban_Pattern_DevOps.png)

![](https://static001.infoq.cn/resource/image/93/8b/93cc49843b45dd772627527c8423228b.png)

## 拓展阅读

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
- [PM指南:项目管理信息系统|工具与技术](https://riboseyim.github.io/2019/04/06/Project-PMIS/)
- [PM指南:项目管理开局模板|工具与技术](https://riboseyim.com/2018/06/19/Project-Template/)
- [PM指南:开源项目管理平台Redmine|工具与技术](https://riboseyim.com/2016/04/26/TeamWork-Redmine/)
- [PM指南:软件业看板Kanban管理实践|工具与技术](https://riboseyim.com/2017/08/06/TeamWork-Kanban/)
- [PM指南:源代码版本管理|工具与技术](https://riboseyim.com/2016/05/31/TeamWork-Git/)

## 参考文献
- [张逸：可视化设计工作坊](http://agiledon.github.com/blog/2014/08/09/visualize-design-workshop/)
