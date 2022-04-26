---
title: 专题：编程语言系列
date: 2017-05-26 10:37:03
tags: [Developer,DevOps]
categories: 工程技术
---
## 摘要
- 如何选择编程语言
- Stack Overflow：2017年最赚钱的编程语言

>程序员非常 **忠于** 他们心爱的语言。编程语言与其说它是技术，还不如说是程序员的思考模式。编程语言是技术和宗教的混合物。——《黑客与画家》（Hackers & Painters）

<!-- more -->

## 论编程方法

>应用软件运行速度提升的关键在于有一个好的性能分析器(profiler)帮助指导程序开发。《黑客与画家》（Hackers & Painters）(p165 )

- [关于编译器（Compiler）& 编译原理](https://riboseyim.github.io/2017/05/12/Compiler/)


## 玩转编程语言

- [玩转编程语言：C 语言](https://riboseyim.github.io/2019/01/07/Language-C-lang/)
- [玩转编程语言：Go 语言](https://riboseyim.github.io/2017/05/05/Language-Go-lang/)
- [玩转编程语言：Perl 语言](https://riboseyim.github.io/2017/05/16/Language-Perl-lang/)
- [玩转编程语言：Python 语言](https://riboseyim.github.io/2017/06/19/Language-Python-lang/)
- [玩转编程语言：Node.js 语言](https://riboseyim.github.io/2017/05/16/Language-Node-lang/)
- [玩转编程语言：R 语言](https://riboseyim.github.io/2017/05/16/Language-R-lang/)
- [玩转编程语言：Ruby 语言](https://riboseyim.github.io/2017/05/16/Language-Ruby-lang/)
- [玩转编程语言：Java 语言](https://riboseyim.github.io/2019/01/07/Language-Java-lang/)
- [玩转编程语言：Lua 语言](http://riboseyim.github.io/2018/08/23/Language-Lua-lang/)
- [玩转编程语言：Erlang 语言](https://www.erlang-solutions.com/blog/twenty-years-of-open-source-erlang.html#disqus_thread)
- [What is the difference between a wrapper, bindings and a port?](https://stackoverflow.com/questions/8628326/what-is-the-difference-between-a-wrapper-bindings-and-a-port)
- [A graph of programming languages connected through compilers ](https://akr.am/languages/)

![](http://riboseyim-qiniu.riboseyim.com/Language_Compilers.png)

![](http://riboseyim-qiniu.riboseyim.com/Language_Compiler_Java_C.png)

#### 关注
- [Will Julia Replace Python and R for Data Science?](https://dimensionless.in/will-julia-replace-python-and-r-for-data-science/)

## 如何选择编程语言

在不考虑其他因素的情况下，总的来看，对于应用程序来说，**选择总体上最强大、效率也在可接受范围内的编程语言**。

如果从 **图灵等价（Turing-equivalent）** 的角度看来看，所有语言都是一样强大的，但是这对程序员没有意义。关于强大很难正式定义，有一个解释方法是一些功能在一种语言是内置的，但是在另一种语言中需要修改解释器才能做到，那么前者就比后者更强大。

如果A语言有一个运算符可以移除字符串中的空格，而B语言没有这个运算符，这种情况则不足以称A语言比B语言强大，因为你可以在B语言里写一个函数实现这个功能。但是A语言支持某种高级功能（假定是递归），而B语言不支持，你就不可能通过自己编写函数库解决了，这就代表A语言比B语言更强大。

- 例外情况：
1）如果在开发的程序必须与另一个程序紧密配合，那么可能最好还是使用后者的开发语言。
2）如果程序只是要做一些很简单的事（比如整数运算或位操作），那就不妨使用一种比较靠近机器的低层次语言，这样运行起来会更快一些。
3）如果程序只是为了特定场合一次性使用，那么你最好根据自己需要解决的问题选择具有强大函数库的语言。

## 论编程方法

>书上说，调试（debugging）是最后的步骤，用来纠正打字的错误和疏忽。可是我的工作方法看上去却像编程就是在调试。编程语言是用来帮助思考程序的，而不是用来表达你已经想好的程序。它应该是一支铅笔，而不是一支钢笔。《黑客与画家》（Hackers & Painters）(p22)

>不要把编程语言看成那些已完成的程序的表达方式，而应该把它理解成促进程序从无到有的一种媒介。这里的意思是说，成品的材料和开发时用的材料其实是不一样的。搞艺术的人都知道，这两个阶段往往需要不同的媒介。比如，大理石是一种非常良好、耐用的材料，很适合用于最后的成品，但是它极其缺乏弹性和灵活性，所以不适合在构思阶段用来做模型。《黑客与画家》（Hackers & Painters）（p215）

## Stack Overflow：2017年最赚钱的编程语言

- Source: [The Highest Paying Programming Languages in 2017](https://www.stackoverflowbusiness.com/blog/the-highest-paying-web-development-languages-in-2017)

Post By [Rich Moy](https://www.linkedin.com/in/richardmoy/): a Content Marketing Writer and Developer Hiring Expert at Stack Overflow.

#### Top-Paying Languages in the US

- [Go](https://riboseyim.github.io/2017/05/05/Go/) - $110,000
- Scala - $110,000
- Objective-C - $109,000
- CoffeeScript - $105,000
- [Perl](https://riboseyim.github.io/2017/05/16/Language-Perl-lang/) - $105,000
- C++- $100,890
- [R](https://riboseyim.github.io/2017/05/16/Language-R-lang/) - $100,000
- Swift- $100,000
- TypeScript- $100,000
- Python- $99,000

根据Stack Overflow 的统计数据，2009至2017之间, Go程序员的报酬增长了 1200% ， Scala 增长了 1250%。注意：单位是 $ ，刀 ,美刀 ！
![](http://riboseyim-qiniu.riboseyim.com/Language_Paying_GO_2017.png)

![](http://riboseyim-qiniu.riboseyim.com/Language_Paying_Scala_2017.png)

#### Top-Paying Languages in Worldwide

- Clojure - $72,000
- Rust - $65,714
- Elixir - $65,000
- F# - $64,516
- Go - $64,516
- Perl - $63,068
- Groovy - $61,809
- Ruby - $60,000
- Scala - $60,000
- R - $57,125

Clojure（发音类似"closure"）是一套现代的Lisp语言的动态语言版。它是一个函数式多用途的语言。Clojure可以执行于Java虚拟机，通用语言运行时以及JavaScript引擎之上,拥有复杂的宏。Clojure第一个稳定版本1.0于2009年5月4日发布，最新的稳定版本是1.8（2016-01-19日）。应用案例：沃尔玛仓储数据库系统、Web新闻和博客信息分析（Chartbeat）、设计开发和基础设施管理工具（8th Light 、Puppet）等，更多 >>>> https://clojure.org/community/success_stories。

Elixir是一个基于 Erlang 虚拟机的函数式、面向并行的通用编程语言。可通过宏实现元编程对其进行扩展，并通过协议支持多态，目标是在维持与现有Erlang工具链及生态环境兼容性的同时，让人们可以在Erlang虚拟机上进行扩展性更好的、高生产率的开发。发行时间 2012年，最新版本 1.4.0（2017年1月5日）。

#### 你可能是一个假的程序员
很多同学看了这个排行榜是不是已经跃跃欲试要去美帝捞金了呢？且慢，最好考虑一下能不能通过海关和移民局的审查先。年初，有一位尼日利亚老兄到美帝打短工就差点被当作非法移民遣返：

>Questions to ask a software engineer:
“Write a function to check if a Binary Search Tree is balanced.”  
海关官员：来，走一个，检查二叉树是否平衡，手写代码.....
小哥：What....
>“What is an abstract class, and why do you need it?”
海关官员：什么是抽象类，你为什么要使用它?
小哥卒

![](http://riboseyim-qiniu.riboseyim.com/Language_Program_US_IM_Twitter.png)

![](http://riboseyim-qiniu.riboseyim.com/Language_Pragram_US_IM_Discuss.png)

## 社交语言

- [WHICH IS THE BEST LANGUAGE TO LEARN?](https://www.1843magazine.com/content/ideas/robert-lane-greene/which-best-language-learn)
Once a mark of the cultured, language-learning is in retreat among English speakers. It’s never too late, but where to start?

## 参考文献
- [每个程序员书柜必备的编程书籍 |原创 2016-11-04 Dan Luu、刘志勇 InfoQ](https://mp.weixin.qq.com/s/ZMsVmkTqx9asr4-T8t6wvA)
- [A Map Showing How Much Time It Takes to Learn Foreign Languages: From Easiest to Hardest](http://www.openculture.com/2017/11/a-map-showing-how-much-time-it-takes-to-learn-foreign-languages-from-easiest-to-hardest.html)
- [A software engineer is detained for several hours by U.S. Customs — and given a test to prove he’s an engineer](https://www.linkedin.com/pulse/software-engineer-detained-several-hours-us-customs-given-fairchild)
- [新华社：入境美国还要先答题？这个程序员差点吃闭门羹](http://news.xinhuanet.com/world/2017-03/03/c_129500094.htm)
- [Julia Evans：Why I love Rust](https://jvns.ca/blog/2016/01/10/why-i-rust/)
