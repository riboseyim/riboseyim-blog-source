---
title: 玩转编程语言:编程技巧
date: 2016-05-05 15:09:21
tags: [Developer]
categories: 工程技术
---
## 摘要
- 1.1 如何阅读源码？
- 1.2 程序命名规范

>If you're doing an experiment, you should report everything that you think might make it invalid - not only what you think is right about it. —— Richard Feynman

<!--more-->

## 一、如何阅读源码？

#### 1.1 阅读源码的一般流程
1. 划分核心子系统（eg: Linux kernel 进程管理子系统、文件管理子系统）
2. 分析主要数据结构 （结构体）
3. 关键程序列表(加载顺序,调用关系call flow,消息传递路径，性能消耗）
4. 主题式探索、带着目的分析实例（eg: Linux 支持闰秒吗？）

> 技能提高的秘诀：迫于需要的学习。

|Problem Domain|Model|Arch & Implement|Improvement|Best Practice|
|-----|-----|-----|-----|-----|
|项目、框架和关键性概念 Architecture Key Concept |-----|问题模型化、验证Demo|-----|-----|

框架为应用而生：平衡技术实现、业务应用、项目

#### 1.2 程序命名规范

>The most important consistency rules are those that govern naming. The style of a name immediately informs us what sort of thing the named entity is: a type, a variable, a function, a constant, a macro, etc., without requiring us to search for the declaration of that entity. The pattern-matching engine in our brains relies a great deal on these naming rules.

>Naming rules are pretty arbitrary, but we feel that consistency is more important than individual preferences in this area, so regardless of whether you find them sensible or not, the rules are the rules.

- [Google Code Style 笔记 - Simple Code Style](https://github.com/riboseyim/simple-code-style)


## 二、编程工具与开发自动化

- [API 文档神器 Swagger 介绍及在 PHP 项目中使用 | Leo108's Blog](https://leo108.com/pid-2296/)

## 参考文献
- [张逸：代码的体格](http://agiledon.github.com/blog/2014/12/16/the-figure-of-code/)
- [张逸：高质量代码的特征](http://zhangyi.farbox.com/post/coding/feature-of-high-quality-code)
- [【翻译】设计线程安全的数据结构的几条准则 | 选自《C++ concurrency in action》chapter 6.2](http://cholerae.com/2014/12/30/-%E7%BF%BB%E8%AF%91-%E8%AE%BE%E8%AE%A1%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E7%9A%84%E5%87%A0%E6%9D%A1%E5%87%86%E5%88%99/)
- [The Universal Design Pattern | STEVE YEGGE KIRKLAND, WASHINGTON, US ](http://steve-yegge.blogspot.com/2008/10/universal-design-pattern.html)
- [Tests Are About Design | Karl Seguin ](http://openmymind.net/Tests-Are-About-Design/)
- [Software Craftsmanship: Testing | Karl Seguin](http://openmymind.net/Software-Crafstmanship-Testing/)
- [邱俊涛：如何成为一名优秀的程序员？](http://abruzzi.github.com/2017/07/tips-for-newbies/)
- [邱俊涛：为什么优秀的程序员喜欢命令行](http://abruzzi.github.com/2017/01/why-top-programmers-hate-gui/)
#### 关于重构
- [如何重构“箭头型”代码 | 酷壳](http://coolshell.cn/articles/17757.html)


#### Catalog:[Language,开发语言](https://riboseyim.github.io/2017/05/26/Language/)

[source:https://riboseyim.github.io/](https://riboseyim.github.io/)
