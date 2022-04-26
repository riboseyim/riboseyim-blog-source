---
title: DevOps 漫谈：DevOps实践的本质是文化
date: 2018-03-29 09:21:17
tags: [DevOps,Engineering]
categories: 工程技术
---
## 摘要
- DevOps实践的本质是文化

<!--more-->

## DevOps实践的本质是文化

2016-07-24 运维日 广州活动纪要
地点：羊城晚报 同创汇

#### 刘宇

职业路径：新浪－>Puppet实践－>Puppet系列文集－>InfoQ编辑－>西山居
社区：LinuxTone.org、InfoQ

- Automation：工具化
- Lean 精益：
- Measurement：度量一切
- Sharing：不管是否成功

- 人物：Patrick Debois：比利时，独立顾问，DevOps领袖（2008年）

**推荐书单**
- [《The Phoenix Project》](https://riboseyim.github.io/2018/04/10/DevOps-Phoenix/)
- 《The DevOps Cookbook》

- 企业：Facebook
>Don’t make me think

**凡是被很多人不断重复的好习惯，要将其自动化整合到工具**

- 实践经验
1.站立会
2.[Kanban|看板](https://riboseyim.github.io/2017/08/06/TeamWork-Kanban/)
3.复盘：总结－发现问题－改进
4.[带领团队翻译书籍 （这事听着都打鸡血)](https://riboseyim.github.io/2017/06/27/Technology-English/)

>学习力－团队生命之根

#### 魅族云

1.异地多活：专线打通多个IDC机房
2.KVM
3.网络：OpenStack 实现控制器、GSLB 全局负载均衡，智能路由

#### 微信：自动压力测试实践
张兴：8年＋

1.生产环境运行压力测试
2.接入Proxy层，调整请求分配权重
3.压测管控

失败监控（系统、逻辑）
耗时监控（SUR client）
快速拒绝（如何判断是否可以丢弃？）
硬件限制（CPU，内存，网卡）

数据：
15s监控数据入库画图，
1min回退配置（1w台服务器），
降低放量速率

#### YY安全

- 案例一：海量小包攻击性防御
1.20140828  脉冲DDoS攻击
40小时，40人团队，监控不足
5秒钟一次，发送小包

黑产报价：150元可购买 15G流量，700W pps，攻击成本低

- 案例二：合作方服务器被入侵概率高（木马）

ps/netstat污染处理：内核态，查看System.Call  (可以挖掘写一篇)

- 案例三：主播IP泄漏
业务逻辑bug，异常抛出IP

- 案例四
确定当前主要矛盾：投诉盗号、盗刷？服务器入侵？DDoS ?
利用现有资源（自有编制、第三方合作），最大投入产出比

- 决策变量
技术预研、业务规模、发展阶段、主要矛盾（切入点）

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

## 参考文献
- [中小企业监控体系构建实战](https://mp.weixin.qq.com/s?__biz=MzA4Nzg5Nzc5OA==&amp;mid=400138875&amp;idx=1&amp;sn=01b4ea2978370d215442e4a22d7d2a7f&amp;scene=1&amp;srcid=1028tfQzmCsfXfTybP9dWEAy#rd)
- [LinkedIn:如何利用大数据算法定位网站性能瓶颈(BOSS)](https://mp.weixin.qq.com/s?__biz=MzAwMDU1MTE1OQ==&mid=2653548227&idx=1&sn=3deb458aa18fa878dc9eaf89aaa78303&chksm=813a7f5bb64df64d7037213ef34b3ff6c09375593ec82919b214829b825e82565b6058366ef1&mpshare=1&scene=1&srcid=01190z58NNndr7YlKBPp5Jz6#rd)
- [树莓派(raspberrypi)、saltstack 在线下自助机运维上的应用 | Jasey Wang](http://jaseywang.me/2017/02/08/%e6%a0%91%e8%8e%93%e6%b4%beraspberrypi%e3%80%81saltstack-%e5%9c%a8%e7%ba%bf%e4%b8%8b%e8%87%aa%e5%8a%a9%e6%9c%ba%e8%bf%90%e7%bb%b4%e4%b8%8a%e7%9a%84%e5%ba%94%e7%94%a8/)
- [邱俊涛：为故障和失败做设计](http://abruzzi.github.com/2016/05/design-for-failure/)
- [酷壳：AWS 的 S3 故障回顾和思考](http://coolshell.cn/articles/17737.html)
- [酷壳：从GITLAB误删除数据库想到的](http://coolshell.cn/articles/17680.html)
- [酷壳：这多年来我一直在钻研的技术](http://coolshell.cn/articles/17446.html)
- [GitHub运维机器人](http://www.infoq.com/cn/presentations/chatops-at-github)
- [运维的本质：可视化 |2015-05-14 王津银 InfoQ](https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&amp;mid=206585329&amp;idx=1&amp;sn=b3cdb637dfff9b4378ec703c9b27a331&amp;scene=1%23rd)
