---
title: 如何度量复杂性
date: 2018-03-25 20:05:04
tags: [数学与算法,Engineering]
categories: 工程技术
---
## 摘要
- 复杂系统、复杂度(Complexity)
- 如何度量复杂度：熵、算法信息量、逻辑深度、热力学深度、计算能力、层次性

<!--more-->

## 如何度量复杂度

2001 年，物理学家劳埃德（Seth Lloyd）发表了一篇文章（《Measures of complexity：A non-exhaustive list.  IEEE Control Systems Magazine, August 2001 》），分别从动力学、热力学、信息论等方面列出了 40 种度量复杂性的方法。提出了度量一个事物或过程的复杂性的三个维度：
- 描述它有多困难 ？
- 产生它有多困难 ？
- 其组织程度如何 ？

#### 用熵度量复杂性
**香农熵**，相对于信息接收者的平均信息量或“惊奇度”。例如，假设消息由符号 A、C、G 和 T 组成。如果序列高度有序，如“A  A  A  A  A  A …… A ” 则熵为零。完全随机的序列则有最大可能熵。最复杂的对象不是最有序的或最随机的，而是介于两者之间。

在英语里，信息和情报是同一个词（Information），而我们知道情报的作用就是排除不确定性。

>如果没有信息，任何公式或者数字的游戏都无法排除不确定性。这个朴素的结论非常重要，但是在研究工作中经常被一些半瓶子醋的专家忽视，希望做这方面工作的读者谨记。(吴军 |《数学之美》)

用香农熵度量复杂性的问题。首先，所针对的对象或过程需要转换成某种“消息”的形式，这并不总是那么容易做到。另外，随机消息的熵最高，但是那些最复杂的对象都不是完全随机的。例如变形虫的基因长度远远超过其他动植物，具备复杂功能的人类基因也不是随机的，甚至能看到长期历史演化出来的规律。

#### 用算法信息量度量复杂性
为了改进熵度量复杂性的方法，1997 年柯尔莫哥洛夫（Andrey Kolmogorov）等人提出了算法信息量的概念。他们将事物的复杂度定义为能够产生对事物完整描述的最短计算机程序的长度。

为了计算有效复杂性，首先要给出事物规则性的最佳描述；有效复杂性定义为包含在描述中的信息量或规则集合的算法信息量。事物的结构可预测性越大，有效复杂性就越低。

对于一个事物的各种规则集，如何决定哪个是最好的呢？**奥卡姆剃刀（Occam's Razor, Ockham's Razor）**，意思是简约之法则。最好的规则集是能够描述事物的最小规则集，同时还能将事物的随机成分最小化。奥卡姆剃刀在现代医学中的应用最具代表性，医生和医学哲学家提出了 **症状化约原则（diagnostic parsimony）**。即在诊断某个病症时，医生应该尽量寻找会导致所有症状的最简单可能性。“当你听到背后有蹄声时，应该想到马而不是斑马。”  

虽然症状化约原则常常可能是正确的，然而也有争锋相对的意见：**西卡姆格言（Hickam's dictum）**，“病人乐意得多少种病就能得多少种病。”  从统计数据看，一个病人的多种症状往往会归于多种常见疾病，而不是仅仅一种。另外，排除统计的因素，有许多病人无法使用单一疾病来解释众多的症状，被证实患有多种疾病。实际上，医生很难根据一个症状来判断是或者不是某个疾病，而是不断地根据病人的生活环境、日常习惯、用药史等等来建立、测试和修改对于症状的假设才是更好的方法。

#### 用逻辑深度度量复杂性
为了更加接近我们对复杂性的直觉，数学家班尼特在 20 世纪 80 年代初提出了 **逻辑深度（logical depth）** 的概念。

用班尼特的话说，“有逻辑深度的事物从根本上必须是长时间计算或漫长动力过程的产物，否则就不可能产生。” 或是像劳埃德说的，“用最合理的方法生成某个事物时需要处理的信息量等同于这个事物的复杂性，这是一个很吸引人的想法。”

#### 用热力学深度度量复杂性
20 世纪 80 年代末，劳埃德和裴杰斯（Heinz Pagels）提出了一种复杂性度量方法：热力学深度（thermodynamic depth）。

与图灵机生成对事物的描述所需的时间步不同，热力学深度首先是确定“产生出这个事物最科学合理的确定事件序列”，然后测量“物理构造过程所需的热力源和信息源的总量”。例如，要确定人类基因组的热力学深度，得从最早出现的第一个生物的基因组开始，列出直到现代人类出现的所有遗传演化事件（随机变异、重组、基因复制等）。

问题：如何定义系统的状态粒度，在列出事件时，如何确定相关的宏观状态。

#### 用计算能力度量复杂性
一种观点认为，系统的计算能力如果等价于通用图灵机的计算能力，就是复杂系统。不过，班尼特等人则认为，具有执行通用计算的能力并不意味着系统本身就是复杂的；应当测量的是**系统处理输入时的行为的复杂性**。

#### 用层次性度量复杂性
如何用层次（degree of hierarchy）来刻画一个系统的复杂性：复杂系统由子系统组成，子系统下面又有子系统，不断往下。（《复杂性的结构》：The architecture of complexity. Simon 1962）

西蒙认为，复杂系统最重要的共性就是 **层次性** 和 **不可分解性**。在层次性复杂系统中，子系统内部的紧密相互作用比子系统之间要多得多。例如，细胞内部的新陈代谢网络就比细胞之间的作用要复杂得多。

## 总结
各种度量复杂性的方法都抓住了一些方面，但都存在理论上和实践上的局限性，还远不能有效刻画实际系统的复杂性，度量方法的多样性也表明复杂系统具有许多维度，无法用单一的尺度来刻画。

## 扩展阅读
- [Machine Learning:机器学习算法](https://riboseyim.github.io/2018/02/10/Machine-Learning-Algorithms/)
- [Machine Learning:经济学家谈人工智能](https://riboseyim.github.io/2018/03/09/Machine-Learning-Economist/)
- [数据可视化（三）基于 Graphviz 实现程序化绘图 | 开源中国首页推荐·每日一博](https://riboseyim.github.io/2017/09/15/Visualization-Graphviz/)

## 参考文献
- 2001 劳埃德（Seth Lloyd）《Measures of complexity：A non-exhaustive list.  IEEE Control Systems Magazine, August 2001 》
- 1962 Simon《复杂性的结构》：The architecture of complexity.
- [如何测出一句话中的信息量](https://www.guokr.com/article/172437/)
