---
title: 概率论基础：从马尔可夫模型（Markov Model）到贝叶斯网络
date: 2017-05-09 16:16:55
tags: [数学与算法]
categories: 工程技术
---
## 摘要
- 马尔可夫模型（Markov Model ）
- 隐含马尔可夫模型（ Hidden Markov Model ）
- 隐含隐马尔可夫模型的应用
- 马尔可夫链的扩展 —— 贝叶斯网络（Bayesian Network）

<!--more-->

19 世纪到 20 世纪初，**马尔可夫（Andrey Markov）提出假设**，任意一个词 Wi 出现的概率只同它前面的词 Wi-1有关，偷懒但颇为有效的方法。【符合这个假设的随机过程则称为马尔科夫过程，也称为马尔可夫链。】二元模型（ Bigram Model ）对应的公式如下：

 P(S) = P(W1 ,W2, … , Wn)
	=P(W1)·P(W2|W1) ·P(W3|W2) ··· P(Wi | Wi-1) ··· P(Wn | Wn-1)

- 马尔可夫链：符合这个马尔可夫假设的随机过程则称为马尔科夫过程。

```go
digraph markov{
  label = " Markov 马尔可夫链";
  rankdir = LR;
  m1->m2[label = "1.0"];
  m2->m3[label = "0.6"];
  m3->m4[label = "0.3"];
  m2->m4[label = "0.4"];
  m3->m3[label = "0.7"];
}

```

![](http://riboseyim-qiniu.riboseyim.com/markov.png)

- 隐含马尔可夫模型（ Hidden Markov Model ）
最早由20世纪六七十年代，[美国数学家鲍姆（ Leonard E. Baum ）](#) 发表的一系列论文中提出。隐含马尔可夫模型是马尔可夫链的一个扩展，即任一时刻 t 的状态 St 是不可见的。但是，在每个时刻 t 会输出一个富豪 Ot ，而且 Ot 和 St 相关且仅和 St 相关。

![](http://riboseyim-qiniu.riboseyim.com/markov-hidden.png)

## 隐马尔可夫模型的应用
隐含马尔可夫模型成功的应用最早是语音识别（Sphinx——大词汇量连续语音识别系统）。根据应用的不同而有不同的名称,例如语音识别中的声学模型（ Acoustic Model ），机器翻译中的翻译模型（ Translation Model ）等。

#### 1、应用领域：翻译
- 语音识别，声学模型（ Acoustic Model ）
- 机器翻译，翻译模型（ Translation Model ）
- 中文断词/分词或光学字符识别
- 拼写纠错，纠错模型（ Correction Model）
- 手写体识别

#### 2、应用领域：图像识别

#### 3、生物信息学 和 基因组学
- 基因组序列中蛋白质编码区域的预测
- 对于相互关联的DNA或蛋白质族的建模
- 从基本结构中预测第二结构元素
- 通信中的译码过程

### 隐含马尔可夫模型的训练

- 知识背景：概率论
- 围绕隐含马尔可夫模型的基本问题
  1. 给定一个模型，如何计算某个特定输出序列的概率；
    Forward-Backward 算法
    参考书 Frederick Jelinek《Statistical Methods for Speech Recognition(Language, Speech, and Communication)》
  2. 给定一个模型和某个特定的输出序列，如何找到最可能产生这个输出的状态序列
    解码算法：维特比算法
  3. 给定足够量的观测数据，如何估计隐含马尔可夫模型的参数。
    绚练算法：鲍姆-韦尔奇算法（Baum-Welch Algorithm）

## 扩展阅读

- [读书笔记|数学之美（Beauty Of Mathmetics）](https://riboseyim.github.io/2017/08/30/Mathmetics-Beauty/)

## 参考文献
- [马尔科夫模型与离散事件系统仿真模型简析 |  则裕沙龙@立波 ](https://mp.weixin.qq.com/s/1qM_J5We1iSe8_Ty-AL4Sw)
- [从贝叶斯方法谈到贝叶斯网络 | v_JULY_v (结构之法 算法之道)](http://blog.csdn.net/v_july_v/article/details/40984699)
- [斯坦福大学公开课-机器学习过程-马尔可夫决策过程](http://open.163.com/movie/2008/1/2/N/M6SGF6VB4_M6SGKSC2N.html)
- [从贝叶斯定理到概率分布：综述概率论基本定义 | 算法与数学之美](https://mp.weixin.qq.com/s?__biz=MzA5ODUxOTA5Mg==&mid=2652555495&idx=1&sn=f5b46175b9a0cbd4006a193a0ca91782&chksm=8b7e39bcbc09b0aa6d9b32a0d49926090fe7fb160ba4fbd51ed17cbbdb05806b234024198cf4&mpshare=1&scene=1&srcid=0925x4UfKSwzSRQWFceVQEka%23rd)
- [实例详解贝叶斯推理的原理 | 2017-10-03 算法与数学之美](https://mp.weixin.qq.com/s?__biz=MzA5ODUxOTA5Mg==&mid=2652555638&idx=2&sn=efc42acdeaa7921637924463cbf38db0&chksm=8b7e382dbc09b13bc8048c762a088c0ae2b979706465aaef4b454c6289e7161a19b8998f114d&mpshare=1&scene=1&srcid=100303yDRzK0nt5PI8G4NTcd%23rd)
