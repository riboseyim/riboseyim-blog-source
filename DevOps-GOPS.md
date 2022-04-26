---
title: 技术社群：高效运维(GOPS)
date: 2016-09-24 13:31:47
tags: [Linux,DevOps,架构师]
categories: 工程技术
---
## 摘要
- 2016 GOPS 上海站简报
- 扩展阅读：工程师的自我修养之全英文技术学习实践

![](http://riboseyim-qiniu.riboseyim.com/GOPS_201609_Chris.png)

<!-- more -->
## 一、2016 GOPS 上海站简报
#### 1.1 风向：从概念到落地

延续这两年的风向，云计算、大数据、DevOps、容器等依然是比较热闹的话题。
鉴于这些关键词已经比较普及，大家聊的内容开始更多地聚焦在落地应用上。比如关于OpenStack，有人从网络架构角度聊，也有人从数据中心、对象存储、可视化等方面聊。借用李明宇的一句话：“OpenStack是一个生态”，很多公司已经在利用其庞大生态的细分技术来创业了。

![](http://riboseyim-qiniu.riboseyim.com/GOPS_201609_Li.png)

从行业分布来看，除了“传统的”几家巨头互联网公司，GOPS大会已经出现了越来越多的传统企业面孔，可能是上海金融中心的背景，银行、保险公司、银联等金融属性的企业参会的很多，此外中移动、石油系、旅游、快递业也很活跃，还办了中小企业专场。

最后，无论是现场交流环节的观众提问，还是我自己的体会，对于大部分人来说，最难的可能还是选型困境：技术方案不是太少，而是选择太多了。以分布式服务为例，有人推崇Zookeeper，有人选择etcd。前者有JAVA派的支持，后者也不乏Go粉的拥趸，或者有的时候就是拍脑袋决策呢？反正各有各的说法，根本也没个标准。希望以后哪位大牛可以来分享下《技术选型指南》。

#### 1.2 特色：Google SRE 分享
本人最感兴趣的是来自Google SRE的分享。

《Google SRE》一书的作者Chris Jones，主要是介绍了一些方法论方面的东西。看气质不像工程师，倒很有几分大学教授的味道。看过《Google SRE》几位作者的简历，除了工程师这个副业，还在斯坦福搞过什么经济学、国际关系、药学博士、文学、诗歌……，Jones则持有Computer Engineering,Economics,Technology Policy三个学位，长期在工程一线，还有professional engineer认证。这哥们还算是它们中间最接近工程师的啦。

Minghua Ye：印象比较深的是推荐了一堆开源库，command-line flags、google protocol buffer、Logging、Googletest…….也许这就是Google人的套路，每当谈到一个什么问题，要么是XXX发表了论文《XXXX》里有说明，要么就是我们发明了XXX技术／工具作为解决方案，开源代码的地址是：XXXX。

孙宇聪：他是《Google SRE》的 中文版译者，前Google SRE。最大的特点就是自带笑点，看得出活得很欢乐、发自内心的。活动现场很多国内企业的CTO、总监 可是极度缺乏这种欢乐。结合zookeeper，讨论了分布式系统的“太祖长拳”——共识系统的一些故事。

![](http://riboseyim-qiniu.riboseyim.com/GOPS_201609_Sun.png)

#### 1.3 终极命题：人才

大多数人可能并没有注意到，本次活动还有一个主题：人才。
在热闹的开幕会场上，宣布了三件事：

1.“不出所料”，萧帮主终于从原企业离职，全职搞社区创业去了。
2.社区和复旦大学合作，推出了联合培养软件工程硕士的项目。
3.社区和EXIN合作，试水DevOps Master认证培训。

特别是后两件事，我其实不太乐观：**干好很难，干砸很容易** 。
从业人才培养难题、在职人员的进修困境，早已有之，连BAT这样的巨头都时常流露出人才难求的尴尬境地。很多有识之士都曾经大声疾呼，但是商业和社区的冲突，能力和学历的争论，再加上当局和教育市场的避重就轻，这方面一直也没走出什么新路。归根到底，这需要产业结构、人才评价体制的根本提升。

不管怎么样，有些事总是需要有人去摸索吧，祝福。

关于工程师人才的微观定义，腾讯提了个自己的标准，供参考：
业务和架构能力
问题诊断能力
开源组件的改造能力
工具开发能力
运营平台构建能力
数据分析和决策能力
……

但是，个人觉得他们这个标准过于功利，还少那么一丢丢。

比如说，第一天Chris Jones演讲的时候，我内心深处是非常郁闷的。
当时Jones说一句，孙宇聪在一旁翻译一句。这场面像极了总理新闻发布会，演讲人和翻译一句一顿，看着都蛋疼。说实话，很难有多少交流传播价值。可是，话说回来了，你要不翻译，至少80%以上的人还真就听不明白，公开活动毕竟要照顾大多数。托教育部的福，现在全国人民的学历线可谓蒸蒸日上，跟通货一样膨胀。但是从业人员的语言水平，能达到并保持一定水准的，还真没几个。

反省自己，阅读资料的时候，不也经常放着官方文档不读，非要去找些东拼西凑的二手货吗？过去买的参考书，原著比例是多少？也算写过几片技术文章吧，被Google索引的有多少？ 能有多少东西是可以和国际同行交流的？

**跨语言能力绝对是弯道超车的必备利器！**

- [工程师的自我修养：全英文技术学习实践](https://riboseyim.github.io/2017/06/27/Technology-English/)。
严肃地讨论一下技术人员的语言能力问题，希望您读完以后能够有所触动、有所行动、有所裨益。

#### Resources
1. [官方实录和PPT下载]([http://mp.weixin.qq.com/s?\_\_biz=MzA4Nzg5Nzc5OA==&mid=2651662368&idx=1&sn=766cd0f708e9d3a1449f1aac926e27e1&scene=1&srcid=0925yHh4bGlTvdUZ7og6Uulq#rd])
2. [2016上海站日程表](http://gops2016-shanghai-tcwechatshare.eventdove.com/)

## 扩展阅读

#### [DevOps 漫谈系列](https://riboseyim.github.io/2016/07/28/DevOps/)
- [《凤凰项目》：从作坊到工厂的寓言故事](https://riboseyim.github.io/2018/04/10/DevOps-Phoenix/)
- [Kanban 看板管理实践](https://riboseyim.github.io/2017/08/06/TeamWork-Kanban/)
- [DevOps 漫谈：基础设施部署和配置管理](https://riboseyim.github.io/2018/03/26/DevOps-Deployment/)
- [Linux 容器安全的十重境界](https://riboseyim.github.io/2017/11/12/DevOps-Container-Security/)
- [工程师的自我修养：全英文技术学习实践](https://riboseyim.github.io/2017/06/27/Technology-English/)

#### [DevOps 实践的本质是文化](https://riboseyim.github.io/2018/03/29/DevOps-Culture/)
- 学习力－团队生命之根
- 带领团队翻译书籍
- Don’t make me think
- 凡是被很多人不断重复的好习惯，要将其自动化整合到工具
