---
title: Night News 20181201
date: 2018-12-01 22:59:26
tags: [News]
categories: 工程技术
---
## 摘要
- 上线新域名：riboseyim.com
- 恢复:七牛云图床故障
- 发布:写作工具链v5
- 推荐工具:AutoJump
<!--more-->

## 上线一个纯洁的域名

#### 新域名

- 解决七牛云“测试”域名回收导致的图床故障
- 提高墙内用户的访问速度和稳定性

## https://riboseyim.com

#### 恢复：七牛云图床故障

以下图床域名统一更新为：http://riboseyim-qiniu.riboseyim.com 。

```
http://o8m8ngokc.bkt.clouddn.com
http://og2061b3n.bkt.clouddn.com
http://ogtqvs10n.bkt.clouddn.com
http://okkuj60pj.bkt.clouddn.com
http://omaxozji3.bkt.clouddn.com
http://omb2onfvy.bkt.clouddn.com
http://ombx24fbq.bkt.clouddn.com
http://osgiyhxhy.bkt.clouddn.com
http://ot6idm48o.bkt.clouddn.com
http://p11slcnom.bkt.clouddn.com
```

#### 发布：写作工具链v5

![](http://riboseyim-qiniu.riboseyim.com/Writer_Tools_Chain_5.png)

- add 独立域名 https://riboseyim.com [Ribose Yim's Tech Blog] 【腾讯云】
- 故障修复：图床域名更换 xxx.clouddn.com to http://riboseyim-qiniu.riboseyim.com 【七牛云】
- add hexo theme [Cafe](https://github.com/giscafer/hexo-theme-cafe)
- auto syn workflow: from riboseyim.github.io to project riboseyim.com
- add Xmind replace MindManager,201807
- 更多细节请查看 [《我的写作工具链》](https://riboseyim.github.io/2017/06/03/Writing-WriterToolChain/)

```perl
# 主题
$ git clone https://github.com/giscafer/hexo-theme-cafe.git themes/cafe

# 素材链接替换
$ grep 'clouddn.com' ./*.md | awk -F '(' '{print $2}' | awk -F '.com' '{print $1}' > oldomain.log
$ sort -n oldomain.log  | uniq > oldomin.list
$ gsed -e "s/old/new/g" sourcefile  >  targetfile
```

#### 推荐工具:AutoJump

autojump 记录你访问过的文件夹（包括记录访问频率进而调整权重），通过路径 cache 实现文件夹位置快速切换。
了解更多细节请查看 [DevOps 资讯 | 是时候升级你的命令行了](https://riboseyim.com/2018/09/03/Linux-Commands-New/)

- Install

```bash
# source
git clone git://github.com/wting/autojump.git
cd autojump
./install.py or ./uninstall.py
```

- Example

```
aca80164:~ kurui$ j -s
10.0:	/Users/yanrui/project/riboseyim.com
50.0:	/Users/yanrui/project-third/autojump
50.0:	/Users/yanrui/project/riboseyim.github.io/source/_posts
________________________________________

109:	 total weight
3:	 number of entries
0.00:	 current directory weight

data:	 /Users/yanrui/Library/autojump/autojump.txt
aca80164:~ kurui$
aca80164:~ kurui$ cd project/ebook-linuxperfmaster/
aca80164:ebook-linuxperfmaster kurui$ j -s
10.0:	/Users/yanrui/project/riboseyim.com
14.1:	/Users/yanrui/project/ebook-linuxperfmaster
50.0:	/Users/yanrui/project-third/autojump
50.0:	/Users/yanrui/project/riboseyim.github.io/source/_posts
________________________________________

124:	 total weight
4:	 number of entries
14.14:	 current directory weight

data:	 /Users/yanrui/Library/autojump/autojump.txt
aca80164:ebook-linuxperfmaster kurui$
aca80164:ebook-linuxperfmaster kurui$ j post
/Users/yanrui/project/riboseyim.github.io/source/_posts
aca80164:_posts kurui$ pwd
/Users/yanrui/project/riboseyim.github.io/source/_posts
aca80164:_posts kurui$ j -s
10.0:	/Users/yanrui/project/riboseyim.com
22.4:	/Users/yanrui/project/ebook-linuxperfmaster
50.0:	/Users/yanrui/project-third/autojump
52.0:	/Users/yanrui/project/riboseyim.github.io/source/_posts
________________________________________

134:	 total weight
4:	 number of entries
51.96:	 current directory weight

data:	 /Users/yanrui/Library/autojump/autojump.txt
aca80164:_posts kurui$

```
