---
title: PM指南:源代码版本管理|工具与技术
date: 2016-05-31 17:37:04
tags: [DevOps,Developer,Engineering]
categories: 工程技术
---
## 摘要
- 思路：[《红楼梦》的版本问题 ](https://riboseyim.github.io/2017/08/28/Redology/)
- 技术出版选题（Git）（电子工业出版社 | 2016）

<!--more-->

## [《红楼梦》的版本问题 ](https://riboseyim.github.io/2017/08/28/Redology/)
- 甲戌本（名“至脂砚斋甲戌抄阅再评仍用石头记”）
- 己卯本（名“脂砚斋四阅评过，己卯冬定本”）
- 庚辰本（名“脂砚斋四阅评过，庚辰秋定本”）
- 俄藏本
- 程高本

## 代码分包
- Perl:pm
- Java:jar
- Node.js:npm
- Go:gvm

>you can only really use Git if you understand how Git works.

## Git ABC

#### Install

```bash
wget https://github.com/git/git/archive/{tag}.tar.gz
gunzip {tag}.tar.gz
cd git-{tag}
make configure
./configure --prefix=/usr/local
make
make install
```

#### 初始化
```perl
git clone git://url
git init-db

## config
git config user.name "your name"
git config user.email yourname@email_server
git config core.editor vim
git config core.paper "less -N"
git config color.diff true
git var -l 查看已经设置的配置
```

Git 中的状态：
- HEAD 代表当前最新状态
- tag  某个状态的标签
- SHA1 每个提交的唯一标识

Git 中有四种对象,都由 SHA1 值表示, .git 目录中保存了全部信息。
- blob 文件
- tree 目录
- commit 提交历史
- tag 标签

```perl
# 显示 file 在 HEAD 中的 SHA1 值
git ls-tree HEAD file
# 显示一个 SHA1 的类型
git cat-file -t SHA1
# 显示一个 SHA1 的内容
git cat-file type SHA1

## diff
git diff tag 比较tag和HEAD之间的不同
git diff tag file 比较一个文件在两者之间的不同
git diff tag1..tag2 比较两个tag之间的不同
git diff SHA11..SHA12 比较两个提交之间的不同
git diff tag1 tag2 file or
git diff tag1:file tag2:file 比较一个文件在两个tag之间的不同

## log
git log file 查看一个文件的改动
git log -p 查看日志和改动
git log tag1..tag2 查看两个tag之间的日志
git log -p tag1..tag2 file 查看一个文件在两个tag之间的不同
git log tag.. 查看tag和HEAD之间的不同

## add/delete/ls:
git add -a 添加所有文件(除了.gitignore)
git rm file 从git仓库中删除文件
git commit 添加或是删除后要提交
git ls-files -m 显示修改过的文件
git ls-files 显示所有仓库中的文件

## commit
git commit -a -e 提交全部修改文件，并调用vim编辑提交日志
git reset HEAD^ or
git reset HEAD~1 撤销最后一次提交。
git reset --hard HEAD^ 撤销最后一次提交并清除本地修改
git reset SHA1 回到SHA1对应的提交状态。

## patch:
git format-patch -1 生成最后一个提交对应的patch文件。
git am < patch 把一个patch文件加入git仓库中。
git am --resolved 如果有冲突，在解决冲突后执行。
git am --skip 放弃当前git am所引入的patch。

## conflict:
git merge 用于合并两个分支。
git diff 如果有冲突，直接使用diff查看，
冲突代码用<<<和>>>表示。手动修改冲突代码。
git update-index 更新修改后的文件状态。
git commit -a -e 提交为解决冲突而修改的代码。

## branch:
git branch -a 查看所有分支。
git branch new_branch 创建新的分支。
git branch -d branch 删除分支。
git checkout branch 切换当前分支。-f参数可以覆盖未提交内容。
```

## 技术出版选题一：Git

主要思路：技术、工程实践、文化心理三者关系
1.工具需要和工程实践结合，本身存在不同的应用范式
2.每一种技术实现背后，除了操作手册，还有必要了解背后发明者的故事，
   发明者的动机、哲学观会深入影响到技术实现。
3.一项技术能够落地，还需要与自己所在组织的文化、心理契合

**结果：电子工业出版社：2016 选题阶段枪毙**

## 第一部分 玩转Git
- 引子1：如何搭建一个免费的博客？
（类似：http://www.jianshu.com/p/fd878edb95e7 ）
 普遍性需求－>抛出问题(内容版本、不仅仅是用于代码管理，建构个人知识库)

- 引子2：引入开源框架遇到问题怎么办？
内容： http://www.jianshu.com/p/8addb7d0024f
（开源社区的正确打开方式）
- 第2章 开启Git之旅

- 2.1 Git生态简介
谁发明？ 谁在用？
- 2.2 10分钟安装指南
Install、First Commit、First Push
部分回答引子的问题

- 2.4 Git客户端工具集

- 2.5 Git服务端工具集

#### 第二部分 源码解读

-  第3章 数据结构
- 3.1 文件
- 3.2 库
- 3.3 分支
- 3.2 标签

- 第4章 核心动作
- 4.1 add
- 4.2 commit
- 4.3 push
- 4.4 merge （调用关系）

- 第5章 从0到1: Git源码演进
- 5.1 第一版Git什么样

#### 第三部分 工程应用

- 第6章 到底要不要使用分支？
- 6.1 个人开发者范式（最简单的入门应用范式）
- 6.2 单一产品团队范式
- 6.3 开源社区范式：以Linux开发模式为例
- 6.4 多项目团队范式

- 第7章 到底要不要分库？
- 7.1 分库的一般原则
- 7.2 极客模式（参考资料：Google所有代码都在一个库）

- 第8章 权限控制问题
企业应用的突出问题： Git为什么不支持代码分权分域？
发明者：自由软件原教旨主义 & 开源社区
了解冲突、选型过程、企业适配

#### 第四部分 附录
[《Linus Torvalds TED访谈实录：Linux和Git的故事》](http://www.jianshu.com/p/841862adb059)

（为什么会有Git,Git与Linux的渊源）

#### Git源码解读  

- Git data model

- Repository
- Branch
- Merge
- Collaborate

可选论题：Git为什么不支持代码分权分域：作者是自由软件原教旨主义者；源于Linux开发模式

## 竞品分析
- [《Git团队协作》](http://www.ituring.com.cn/book/1779?mc_cid=1b81b758f0&mc_eid=b24726accf)

## Tips

```bash
$ git pull origin master
From github.com:riboseyim/go-hello
 * branch            master     -> FETCH_HEAD
fatal: refusing to merge unrelated histories
$ git pull origin master --allow-unrelated-histories
```

## 扩展阅读：[DevOps 漫谈系列](https://riboseyim.github.io/2016/07/28/DevOps/)
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
- [git log – the Good Parts](https://zwischenzugs.com/2018/03/26/git-log-the-good-parts/)
- [为什么美术和策划在使用 git 时会遇到更多麻烦 | 云风的BlOG](http://blog.codingnow.com/2017/04/git.html)
- [《Understanding Git Conceptually》](https://www.sbf5.com/~cduan/technical/git/)
- [Git内部原理](http://www.open-open.com/lib/view/open1328070620202.html#articleHeader5)
- [Git作者之一Scott Chacon的站点](http://schacon.github.io/)
- [(企业实践，包含代码治理有关内容)Python向来以慢著称，为啥Instagram却唯独钟爱它？](https://mp.weixin.qq.com/s?__biz=MzI4MTY5NTk4Ng==&mid=2247489377&amp;idx=1&amp;sn=c6f2101b46bb8534746cb9ae05a4b35f&source=41#wechat_redirect)
