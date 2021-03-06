---
title: Oracle 数据库迁移与割接实践
date: 2016-06-12 09:47:21
tags: [Database,Engineering,DevOps]
categories: 工程技术
---
# 摘要
- 一、项目背景
- 二、主要困难
- 三、迁移前准备
- 四、失败的体验：没有一帆风顺
- 五、总结
<!--more-->

## 一、项目背景
某企业支撑系统，已经连续服务七年有余。算起来比我的工作年限还要长。

历年工程中，硬件、软件、运营团队都更新换了好几茬，单独系统核心数据库—— 一台小型机搭载Oracle 10g，附加一套磁盘阵列，从来没有动过。

随着近年的业务暴涨、负载上升、硬件老化，服务器、磁盘都时有故障发生，负载水平线逼近极限，故障率还有加速抬头的趋势。整个运营团队面临了巨大的客户压力，提升系统稳定性的巨大挑战摆在了大家面前。

<!--![迁移拓扑](http://riboseyim-qiniu.riboseyim.com/oracle-trans-go.png)-->

## 二、主要困难

>在“天塌地陷”的不利局面中加入进来。

#### 2.1 困难1: 团队大动荡

原运营团队，包括但不限于资深项目经理、技术负责人、多名工程师等，先后因各种原因，在很短的时间内集中离职了。在接手之前，对于该项目我一无所知，接手以后短暂的交接过程中，也很难获取多少有价值的信息。

连续发生突发重大故障，我们面临巨大的商务压力，团队内部疲惫不堪、心理压力极大、士气低落。每一次大故障，所有人都得没日没夜地处理，处理好以后还得写材料汇报，汇报之后也未必能得到客户的首肯。甚至在某种程度上说，急剧增长的故障率进一步刺激、增加了离职率。正如一位哲人说的：

>降低故障率是提升团队幸福指数的首要保障。— 弗拉基米.耶维奇.严

#### 2.2 困难2: 拓扑大调整
系统的拓扑结构最初是星型：以数据库和应用服务器为核心，外挂近100台服务器。数据库服务器配置双网卡，连通内网、外网。


![迁移前](http://riboseyim-qiniu.riboseyim.com/oracle-trans-t1.png)

虽然星型结构简单易用，却也存在较大的安全隐患。在早期建设的时候，规范尚未健全，还可以用业务优先的理由搪塞过去。在本期工程中，非常明确必须要完成内外网分离的改造。

<!--![迁移后](http://riboseyim-qiniu.riboseyim.com/oracle-trans-t2.png)-->

#### 2.3 困难3: 安全一票否决
Oracle 版本由10g 升级到 11g，集中管理访问权限。
最大限度地提高安全性，口令60天更换一次，不能因为更换口令中断业务；如果出现连续的错误口令访问，甚至不惜锁定数据库。

从正向的角度看，这些严格规定可以倒逼改造。在软件架构规范化不足、自动化严重不足的条件下，整个迁移期间，这个问题确实给我们造成了极大困扰。

## 三、迁移前准备

基于上述三大问题，在正式迁移之前主要做了下列几项工作：

- 3.1 加强监控手段，降低日常故障率；
	梳理需要监控的基础指标和业务指标，侧重关键业务可用性。例如，某业务的正常调度周期是3小时，部署模拟脚本，将模拟脚本的调度频率提高到5分钟一次甚至更高，通过高密度的执行，主动触发风险点，一些隐藏的问题就比较容易暴露出来。

- 3.2 重点培训新人，稳定队伍；
	本质上说，这次迁移工作的首要任务不在技术、也不在数据，而在于人。
	上一个团队整体流失，新补充的人员又完全没有相关经验，可以说是从0开始。基于该阶段的特殊情况，我选择了实质性暂停迁移工作，而把主要的精力投入到人员培训和组织重建上来。

	关于这块内容比较复杂，实际是另外一个主题，计划后续再发布，敬请关注。

- 3.3 梳理全局视图，调研周边系统关系链；
全局视图实际上也包括技术和人两个维度：

	1）重绘系统架构图：可以参考现有文档资料，但是主要应该立足于自主调研。绘制的过程，即是收集、整理的过程，也是制定迁移方案的思考过程。唯有自己动手，才能加深认识，做到胸有成竹。

	2）大量接触相关方面的领导、配合部门以及第三方厂家：
	个人工作经历方面，独立工作的场景居多，自己能直接控制的情况居多，不太需要理会复杂的部门关系、人际关系。这项工作对于某些人来说比较容易，但是对我而言，其实是有过一段比较困难的过程。

- 3.4 数据复制
主要实现：OGG + dblink+ 自主迁移程序。

**OGG**：即Oracle Golden Gate方案。
在最早的方案中，我们打算完全依赖OGG来实现数据复制，但是在实验阶段发现，该方案有其限制条件。
第一、历史遗留系统有庞大的历史数据，如果都用OGG，无法确保新库的及时性、一致性。
第二、由于管理的不规范，存在很多该做分区而没有做分区的大表，而且这样的表，数量很多，一时还真的很难分离出来。

**DB Link**: 提供了旧－>新库之间的快速连接通道。

**自主迁移程序**：针对特殊大分区表。
例如A表是大量的原始数据，每天一个分区，每个分区约为4000万条记录，一个月就有1.2亿条。由于业务上非常重要，该表的数据必须迁移到新库。针对这个问题，我们自己编写了迁移脚本。按照“少量、多批次、并行”的原则，实现数据推送。

首先，控制每个批次提交的记录数，将每个分区4000万的数据，切分成10万一份的小切片，这样即使失败也能快速重试，还能杜绝UNDO表空间暴涨（例如exp/imp整个分区的方式）。

基于小粒度的切片，进一步就可以实现多批次、多进程的并行推送，从而保证每个commit和时间单元的推送规模都完全可控、可视。

- 3.5 双库并行部署
在所有采集服务器开启两个入库进程，即一份原始结果同时入两套数据库。最大限度在没有额外测试系统的条件下，利用现有资源，模拟仿真正式生产环境的并发压力，同时完成负载均衡、单点故障验证测试。

并不是所有的程序都能轻易的实现双库并行，有的可能只要稍微调整配置文件，有些可能就必须修改代码，还有的可能就没法弄。从这个角度观察，第一种应该就是好的代码，灵活，适应各种可能的场景。后者则可能连配置－代码分离都没有做到，侵入式编程等。

- 3.6 转发入库组件开发
开发这个东西的时候，我一度准备中途放弃。
前两版发布测试的时候，起初都还不错，但是运行一段时间之后，就暴露出严重的性能问题，几乎不可用。多次调试都找不到原因。后来发现，最大的Bug不过是一个空指针异常的捕获不合理。
	很不经意地修改几个字符之后，一切都顺畅起来，往后再也没有出一丁点问题。
	很多时候可能就是这样，需要一点灵性，需要一点运气。

- 3.7 割接工具开发
系统配置收集器（例如口令配置必须保证强一致性）；
转发路径监视（ 外网跳板到内网跳板、跳板到数据库等）；
预配置／检查工具；
连接 切换 & 回退 工具；

![割接流程](http://riboseyim-qiniu.riboseyim.com/oracle-trans-cflow-1.png)

## 四、失败的体验：没有一帆风顺

> 第一次割接失败了！

**失败的体验**

第一次割接之后，系统各项功能正如我们预计的那样顺利运行。
就在我们准备庆祝成功的时候，几项关键业务的吞吐量却急剧下降。初步判断是性能问题，因为每次连接时延飙升了100多倍，高达秒级，基本是不可用的。

人工排查几次以后，看到了大量的挂起进程，一堆的锁表。而且失败来源遍布一多半的服务器。虽然不想承认，但是我们不得不做出回退的决定。

**万事留后路**
割接失败是需要承担很大压力的。
这次割接是大家都期待很久的，为了几分钟的操作，用户和我们整个团队都付出了很大的努力，调动了方方面面的资源参与进来。

如果说有什么能稍感欣慰的话，那就是我们遍历了各种可能，几乎成功，在不可知的情况出现之后，还能赶在割接窗口结束之前，快速回退。这主要得益于前期准备方案时，特别考虑了最坏的情况，认真演练了回退流程。

这种体验与技术关系不大，来自于勇气——无论是正确的，还是错误的决定。

**幽灵进程**
事后筛查发现，导致割接失败的是一个监测程序——不在生产程序清单里面，没有读统一配置，自带定时调度，零散分布在一半的机器上，早已经被人遗忘。在旧库中，这个问题其实一直存在，但是不会有什么问题。

新数据库的版本是Oracle 11g 。
为了提高安全性，防止暴力破解数据库中用户的密码，Oracle默认提供了一种机制：**延长失败尝试响应**。
这种策略是：在连续使用错误密码反复尝试登录时，从第四次错误尝试开始，每次增加1秒的延迟，最长延迟目前是10秒。使用这种手段可以相对比较有效的防治用户密码的暴力破解。

![01](http://riboseyim-qiniu.riboseyim.com/oracle-trans-test1.png)
![02](http://riboseyim-qiniu.riboseyim.com/oracle-trans-test2.png)
![03](http://riboseyim-qiniu.riboseyim.com/oracle-trans-test3.png)

由于系统服务器较多，历史遗留程序瞬间就触发该机制，导致所有应用不可用。


## 五、总结

虽然这次的迁移不甚完美，事情本身也谈不上宏大，简短的一篇更不可能穷举所有的问题和细节，但是有几点思考还是想和大家交流：

- 5.1 变通

	回想起来，开始设计方案时想到的几个大难题，都是通过替代方案实现的：奇葩的内外网分离方案，与IT部门关于权限问题的艰难谈判，数据复制过程中及时性的要求...... 另外，还有真实割接过程中，现场压力状态的进退困境。到处都需要权衡、选择。

	决策是一件非常艰难的事情，受到多种因素的制约，最终的决策是一个各种利益妥协的结果。正如另一位资深哲人所说：

	>项目经理首先要学会变通。

	天下武功，无坚不摧，唯变不破。

- 5.2 韧性
	按照最初的方案，我其实并不负责这项工作，后来就算参与进来，也并不负责主导。应该说起初也有侥幸心理，希望有其他人来背这个锅。为了这次迁移，前面已经生生吓走了好几拨人。

	从技术上分析，我以前没怎么搞过数据库，并不具备任何优势。
	如果说还有一点可以凭借的东西，我感觉是韧性。面对未知的恐惧，敢于直面；面对不确定的方案，不断在尝试；面对失败的后果，坦然接受。

#### [邱俊涛：如何持久化你的项目经历](http://abruzzi.github.com/2016/01/how-to-summarize-privious-project/)
	>从项目上下来之后，需要深入思考并总结之前的经验，这种深入思考会帮助你建立比较完整的知识体系，也可以让你在下一项目中更加得心应手，举一反三。如果只是蜻蜓点水般的“经历”了若干个项目，而不进行深入的总结和思考，相当于把相同的项目用不同的技术栈做了很多遍一样，那和我们平时所痛恨的重复代码又有什么不同呢？

## 参考文献
- [密码延迟验证导致的系统HANG住 | yangtingkun](http://yangtingkun.net/?p=1429)
- [【翻译】为什么我的数据库变得这么慢？ | Cholerae's Blog](http://cholerae.com/2015/01/01/%E3%80%90%E7%BF%BB%E8%AF%91%E3%80%91%E4%B8%BA%E4%BB%80%E4%B9%88%E6%88%91%E7%9A%84%E6%95%B0%E6%8D%AE%E5%BA%93%E5%8F%98%E5%BE%97%E8%BF%99%E4%B9%88%E6%85%A2%EF%BC%9F/)
- [关于Oracle Sharding | Oracle FAQ文档翻译| eygle.com](http://www.eygle.com/archives/2017/03/oracle_sharding.html)
- [防范攻击 加强管控 - Oracle数据库安全的16条军规 | eygle.com](http://www.eygle.com/archives/2017/03/oracle_security_rule_16.html )
- [How to Analyse Row Lock Contention in Oracle 10gR2 and later | kamus](http://www.dbform.com/html/2015/2317.html)
- [iptables高性能前端优化-无压力配置1w+条规则,2017| Bomb250](http://blog.csdn.net/dog250/article/details/77618319)
- [Oracle 数据库健康检查平台](https://mp.weixin.qq.com/s?__biz=MjM5MDAxOTk2MQ==&mid=2650272655&idx=1&sn=9173ca8cdeafa092f1125285909634b2&chksm=be486b99893fe28ffb96e9ae713b91a73b94b71eee0b915c4a5e2c3b75cec39eca0eeb119e79&mpshare=1&scene=1&srcid=1226Q2973ERHUtX4hUpE7tXK#rd)
