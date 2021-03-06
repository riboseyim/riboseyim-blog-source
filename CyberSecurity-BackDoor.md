---
title: Cyber-Security:事与愿违的后门程序
date: 2017-11-06 00:19:12
tags: [Cyber-Security,Policy&Law]
categories: 工程技术
---
## 摘要

一些情报机构赞同在加密软件中放置后门程序，但是谁使用它们呢？

<!--more-->

- 原文：[Internet security | When back doors backfire |Jan 2nd 2016](#)

如果没有加密技术，互联网上传递的信息倒不如写在明信片上。因此，政府、银行和商业机构都对自己的信息加密，当然恐怖组织和犯罪团伙也是如此。后门的问题就是这样，尽管它能让特工的工作更顺利，但也让互联网的每个人都失去了很多秘密。

近期Juniper公司（一家美国网络硬件和软件供应商）事件的启示，清楚地说明了这点。Juniper在12月紧急发布后门公告，最早可追溯到2012年，它使得任何人可以解读基于VPN软件的加密流量（虚拟私有网络），VPN广泛应用于全球的企业和政府机构，以透过公共互联网链接不同办公区。目前不清楚谁该为此事负责，但是可能是在一个情报机构安装后门程序后，其他人又秘密的修改过。该后门涉及的加密标准包含一个有缺陷的随机数生成算法，它获得了美国国家安全局（NSA）的支持；另外一些线索指向中国或英国的情报机构。

针对不同侦查目标破解加密信息显然是情报结构的职责范围，这也是政府结构应当具备监测能力最好理由，当然是在国家安全利益和法律框架的情况下。为了侦查行动而引入后门程序，风险是可能被胡作非为的特工、敌国政府或者犯罪者滥用，而且是在合法的前提下。目前不清楚Juniper的后门程序是什么人安装、使用，结局是什么。

#### who behind
情报机构辩解称：该后门非常隐秘、足够复杂，他们未经授权使用是不可能的。但是外来者可能偶然发现或者窃取到这个漏洞的细节。特别是美国在安全储存保密数据方面的记录非常糟糕。众所周知，在这个夏天，人事管理办公室发生泄密，包括2000万以上联邦雇员（公务员）的敏感个人数据，据称是中国人干的。有人称这一事件为美国情报史上最大的灾难。唯有Edward Snowden 泄密事件可以与之相比，（斯诺登：前国家安全局承包商雇员，目前居住在莫斯科）。（机场安全当局也不小心泄漏了万能钥匙——能打开大多数商业行李——一种物理上的后门）。

#### 反击后门
因此强制性置入后门的观点应当抵制。他们可能被犯罪者利用、从整体上削弱互联网安全，亿万人民赖以进行银行和支付业务。这种行为将损害公众对技术公司的信心，也使西方国家政府对独裁政权干涉互联网的批评更加困难。他们过分的要求在任何情况下都是徒劳的：没有后门的高性能加密软件，任何人在网上都可以免费获得。

与其通过后门程序削弱每个人的秘密，特工应当使用其他手段。

11月巴黎恐怖袭击之所以能成功，不是因为恐怖分子使用了高超的计算机技术，而是因为他们的活动信息没有被有效共享。必要时，国家安全局和其他情报机构通常能用特殊方式在嫌疑人的计算机和手机上植入蠕虫病毒。这比使用一个通用的后门要困难和低效——但是它对别人更安全。

## 扩展阅读

- NSA： National Security Agency
隶属美国国防部（DoD），办公地点在五角大楼，主管是现役军官。和CIA工作的最大不同之处在于，后两者首要的工作是面向“人”的间谍活动，而NSA不能参与此类间谍活动它的典型工作比如监视反越战活动，经济间谍活动，遭斯诺登泄露的公众信息是由NSA负责监控的。

- a new law just passed in China
2015年7月1日，中国全国人大通过了新的《国家安全法》，首次提出“国家建设网络与信息安全保障体系”，“维护国家网络空间主权、安全和发展利益”

- Juniper 漏洞
2015年12月Juniper发布紧急公告，称在对其 NetScreen 产品的内部代码审计过程中发现了两枚漏洞,可以让资深的攻击者获得对 NetScreen 设备的管理权限和解密 VPN 流量。

- VPN 虚拟专用网络
在公用网络上建立专用网络，进行加密通讯。在企业网络中有广泛应用。VPN可通过服务器、硬件、软件等多种方式实现VPN能提供高水平的安全，使用高级的加密和身份识别协议保护数据避免受到窥探，阻止数据窃贼和其他非授权用户接触这种数据。



#### [《The Cyber-Security Master》](https://www.gitbook.com/book/riboseyim/cyber-security-manual)
- [Cyber-Security: 黑客与技术、产业及其精神世界](https://riboseyim.github.io/2017/05/09/CyberSecurity-Hacker/)
- [Cyber-Security: Linux 容器安全的十重境界](https://riboseyim.github.io/2017/11/12/DevOps-Container-Security/)
- [Cyber-Security: 警惕 Wi-Fi 漏洞](https://riboseyim.github.io/2017/10/29/CyberSecurity-WiFi/)
- [Cyber-Security: Web应用安全：攻击、防护和检测](https://riboseyim.github.io/2017/08/31/CyberSecurity-Headers/)
- [Cyber-Security: IPv6 & Security](https://riboseyim.github.io/2017/08/09/Protocol-IPv6/)
- [Cyber-Security: OpenSSH 并不安全](https://riboseyim.github.io/2016/10/06/CyberSecurity-SSH/)
- [Cyber-Security: Linux/XOR.DDoS 木马样本分析](https://riboseyim.github.io/2016/06/12/CyberSecurity-Trojan/)
- [浅谈基于数据分析的网络态势感知](https://riboseyim.github.io/2017/07/14/Network-sFlow/)
- [Packet Capturing:关于网络数据包的捕获、过滤和分析](https://riboseyim.github.io/2017/06/16/Network-Pcap/)
- [新一代Ntopng网络流量监控—可视化和架构分析](https://riboseyim.github.io/2016/04/26/Network-Ntopng/)
- [Cyber-Security: 事与愿违的后门程序 | Economist](http://www.jianshu.com/p/670c4d2bb419)
- [Cyber-Security: 美国网络安全立法策略](https://riboseyim.github.io/2016/10/07/CyberSecurity/)
- [Cyber-Security: 香港警务处拟增设网络安全与科技罪案总警司](https://riboseyim.github.io/2017/04/09/CyberSecurity-CSTCB/)

## 参考文献

- [2018年第三季度网络安全威胁态势分析与工作综述|工信部](http://www.miit.gov.cn/n1146285/n1146352/n3054355/n3057724/n3057734/c6521880/content.html)
宁夏自治区近百万个IP地址发送NTP（Network Time Protocol,网络时间协议）响应信息，疑似被用作DDoS攻击，经宁夏自治区通信管理局组织相关单位研判，确定为宁夏电信公司为家庭宽带用户安装的光调制解调器开启了NTP服务，被攻击者恶意请求后返回的响应信息，宁夏自治区通信管理局督促宁夏电信公司及时对相关用户的近百万台光调制解调器进行远程固件升级，关闭NTP服务，消除被用作发起NTP型DDoS攻击的风险。
