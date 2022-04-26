---
title: 云计算 | Overview of Cloud Computing
date: 2017-07-23 15:21:42
tags: [Economist,DevOps,架构师]
categories: 工程技术
---
## 摘要
- Introducing Cloud Computing
- 云计算平台的选择:AWS or Azure?

<!--more-->

## Overview of Cloud Computing

#### Introduction to Cloud Computing

#### 云计算网络

- Local network（本地网络）
一个只允许在本服务器内通信的虚拟网络，不能跨服务器通信。
主要用于单节点上测试，只能做单节点的部署(all-in-one)，这是因为此网络模式下流量并不能通过真实的物理网卡流出，integration bridge并没有与真实的物理网卡做mapping，只能保证同一主机上的vm是连通的。  

- Flat network
基于不使用VLAN 的物理网络实现的虚拟网络，每个物理网络最多只能实现一个虚拟网络。
ID数只有1个，通常不用于数据网络，因为其不具备租户隔离型。即所有的虚拟机都在一个广播域。比较传统的网络模式，需求较少。

- VLAN network（虚拟局域网）
基于物理VLAN 网络实现的虚拟网络。共享同一个物理网络的多个 VLAN 网络是相互隔离的，甚至可以使用重叠的 IP 地址空间。
在规模不大的私有云中，往往是用 VLAN 模式，简单、可靠、能够满足规模要求；而在大型的私有云或者公有云中，往往使用VxLAN。

- GRE network （通用路由封装网络）
一个使用GRE 封装网络包的虚拟网络。GRE 封装的数据包基于 IP 路由表来进行路由，因此 GRE network 不和具体的物理网络绑定。

#### 云计算存储


## 云计算平台的选择:AWS or Azure?

- 原文链接：**[Stack Overflow:Trends in Cloud Computing: Who Uses AWS, Who Uses Azure?(2017)](https://stackoverflow.blog/2017/07/21/trends-cloud-computing-uses-aws-uses-azure/)**

- 分析师 **[David Robinson](https://github.com/dgrtwo)**：Data Scientist at Stack Overflow, works in R and Python.

- 工具：[The Stack Overflow Trends tool](https://insights.stackoverflow.com/trends)。数据来源（Stack Overflow问题标签，时间跨度从2008至今）: [amazon-web-services](https://stackoverflow.com/questions/tagged/amazon-web-services)、[azure](https://stackoverflow.com/questions/tagged/azure)。

![](http://riboseyim-qiniu.riboseyim.com/Stack_Overflow_Trends_1.png)

#### Traffic to AWS and Azure over time

![Traffic to AWS and Azure over time](http://riboseyim-qiniu.riboseyim.com/Stack_Overflow_Trends_2.png)

很明显，AWS和Azure两个平台在2012的流量水平相当，之后AWS增长得更快。

#### Technologies

![Developers who use Azure and AWS by technology stack](http://riboseyim-qiniu.riboseyim.com/Stack_Overflow_Trends_3.png)

|开发者标签|AWS|Azure|说明|
|-----|-----|-----|-----|
|C#|少|压倒性优势|Azure是微软产品，C#是Window技术栈中Web开发的默认选项|
|Node.js|多|多|Node.js开发者倾向于使用云托管|
|Python／Ruby Rails|多|少||
|C／C++|很少|很少|很少部署在云应用|
|HTML|很少|很少|不负责配置云平台的前端开发人员和设计人员|

#### By industry

![Traffic to Azure and ASW by industry](http://riboseyim-qiniu.riboseyim.com/Stack_Overflow_Trends_4.png)
AWS的问题覆盖广泛，Azure在特定行业，特别是咨询和能源行业广泛使用的平台。进一步分析表明，这些行业是微软堆栈最受欢迎的行业。相比之下，AWS在技术行业（如软件和网络公司）尤其受欢迎，尤其是在媒体公司（包括出版和娱乐）。同样值得注意的是，“学术”部门、主要由学生和研究人员组成，这些用户访问的语言和技术与美国其他地区相比非常不同。这表明云平台在大学里并没有被广泛教授，或者在研究中广泛使用，至少与Python和R等技术相比。

#### By country

![Comparing AWS and Azure traffic by country](http://riboseyim-qiniu.riboseyim.com/Stack_Overflow_Trends_5.png)

大部分国家的访问记录来看，关于AWS的问题数量要多于Azure的问题。但是有一个地方例外——荷兰，关于Azure的问题数量是AWS的两倍，一种可能的解释是微软在荷兰建了一个大型数据中心，导致该地区有相对较多的Azure平台开发者。[Microsoft has built a major data center in the Netherlands](http://www.datacenterdynamics.com/content-tracks/design-build/microsofts-2bn-netherlands-data-center-revealed/96753.fullarticle)

## 参考文献
- [Stack Overflow:Trends in Cloud Computing: Who Uses AWS, Who Uses Azure?(2017)](https://stackoverflow.blog/2017/07/21/trends-cloud-computing-uses-aws-uses-azure/)
- [阿里云MySQL及Redis灵异断连现象：安全组静默丢包解决方法 |纤夫张  数据和云  2月25日](https://mp.weixin.qq.com/s/uuRa-UtzEA7ZwBPcHXnAzg)
- [一文读懂常见云计算网络 | 原创： 杨武  新钛云服  2月15日](https://mp.weixin.qq.com/s/G-H_ECkOJh9HZzZn0B5-5A)
