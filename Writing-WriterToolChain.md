---
title: 我的写作工具链
date: 2017-06-03 16:40:13
tags: [Writing,工具癖,eBook,Mac,Engineering]
mathjax: true
categories: 出版物
---
## 摘要

>工欲善其事，必先利其器

<!--more-->

## Question

- **OmniFouse**: 项目和上下文分类设置还比较混乱，有待重构
- **Ulysses**：多种样式效果不佳（图片连接、代码块）
- **编辑发布流程**:目前的方式（local->Blog(markdown),review->微信公众号），内容粘贴到公众号编辑器还需要比较多手工处理，未统一微信发布内容样式表。
- **Mac**:如何设置左手 <- -> 方向键 ？ （右手仍然需要鼠标的情况下）
- **PPT**:vm win下正常，mac下英文字母乱码
- [Texture：一个优雅的开源学术论文书写工具](https://mp.weixin.qq.com/s/6S2HCDSx9ePwycHcve1Hlw)
- [Classic Papers:谷歌学术推出“经典论文”排行](https://mp.weixin.qq.com/s/hwUKCkpvIjIDeEVKh5xF7g)
- 失效链接（图片、外部链接）自动检测

## History

### v6 （ Current ）

#### 项目管理工具

- [PM指南:网络计划技术|工具与技术](https://riboseyim.com/2019/05/29/Project-Tech-NetworkPlanning/)
- [PM指南:项目管理信息系统|工具与技术](https://riboseyim.com/2019/04/06/Project-PMIS/)

#### 文献管理工具

- 文献管理软件 [Papers](https://www.papersapp.com)

#### 网站管理工具

- 在线截屏工具 [screendump](https://screendump.techulus.com/)

  用户只要输入网址，就会显示各种设备的网页截屏。

#### 写作编辑工具

- **LaTex 语法**:全面增强数学公式支持

- add Atom plugin : markdown-preview-enhanced [预览、提纲、公式增强]
- add hexo kramed replace hexo marked,enable mathjax
- Latex Style 单行公式： $s=a+b$
- Latex Style 多行公式：

$$\frac{\partial u}{\partial t}
= h^2 \left( \frac{\partial^2 u}{\partial x^2} +
\frac{\partial^2 u}{\partial y^2} +
\frac{\partial^2 u}{\partial z^2}\right)$$

- KaTeX Style (markdown-preview-enhanced)：
\[
  E=mc^2
\]

- disable hexo gitment comment plugin
- add 留言箱 https://github.com/riboseyim/riboseyim.com.comment

![](http://riboseyim-qiniu.riboseyim.com/Atom-Markdown-en.png)

- Styled Terminal Markdown Viewer

mdv 在终端下渲染出 Markdown 文档的样式，包含多个主题、支持表格、源代码高亮显示、文件更改监视等功能。

- [project mdv github link](https://github.com/axiros/terminal_markdown_viewer)

```sh
$ pip install mdv
```

![](http://riboseyim-qiniu.riboseyim.com/Tools-Preview-mdv.png)

### v5:20181124

![](http://riboseyim-qiniu.riboseyim.com/Writer_Tools_Chain_5.png)

#### 域名管理工具

- add 独立域名 https://riboseyim.com [Ribose Yim's Tech Blog] 【腾讯云】
- 故障修复：图床域名更换 xxx.clouddn.com to http://riboseyim-qiniu.riboseyim.com 【七牛云】

#### add hexo theme [Cafe](https://github.com/giscafer/hexo-theme-cafe)
- fix hexo search feature -> google search
- add gitment comment
- auto syn workflow: from riboseyim.github.io to project riboseyim.com
- add Xmind replace MindManager,201807
- add slides maker:landslide | 试用 [hacker slides](https://github.com/msoedov/hacker-slides)

```perl
# 主题
$ git clone https://github.com/giscafer/hexo-theme-cafe.git themes/cafe

# 素材链接替换
$ grep 'clouddn.com' ./*.md | awk -F '(' '{print $2}' | awk -F '.com' '{print $1}' > oldomain.log
$ sort -n oldomain.log  | uniq > oldomin.list
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

$ gsed -e "s/old/new/g" sourcefile  >  targetfile
```

```bash
$ landslide Machine-Learning-Algorithms.md -i -o > test.html && open test.html
```

```python
## pip 安装
$ sudo pip install landslide
The directory '/Users/yanrui/Library/Caches/pip/http' or its parent directory is not owned by the current user and the cache has been disabled. Please check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
The directory '/Users/yanrui/Library/Caches/pip' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
Collecting landslide
Requirement already satisfied: Markdown in /usr/local/lib/python2.7/site-packages (from landslide) (2.6.11)
Requirement already satisfied: Jinja2 in /usr/local/lib/python2.7/site-packages (from landslide) (2.10)
Requirement already satisfied: docutils in /usr/local/lib/python2.7/site-packages (from landslide) (0.14)
Requirement already satisfied: Pygments in /usr/local/lib/python2.7/site-packages (from landslide) (2.2.0)
Requirement already satisfied: six in /usr/local/lib/python2.7/site-packages (from landslide) (1.11.0)
Requirement already satisfied: MarkupSafe>=0.23 in /usr/local/lib/python2.7/site-packages (from Jinja2->landslide) (1.0)
Installing collected packages: landslide
Successfully installed landslide-1.1.3

## 源码安装
$ git clone https://github.com/adamzap/landslide.git
$ cd landslide
$ python setup.py build
$ sudo python setup.py install
```

### v4:201801

![](http://riboseyim-qiniu.riboseyim.com/Writer_Tools_Chain_4.png)

#### 可视化图表
- [数据可视化（五）基于网络爬虫制作可视化图表](https://riboseyim.github.io/2017/05/12/Visualization-Charts/)

#### 程序化内容生成

- [基于 Node.js 实现程序化文本编辑](https://riboseyim.github.io/2017/12/21/Language-Auto-Generator/)
- [基于 Graphviz 实现程序化绘图](https://riboseyim.github.io/2017/09/15/Visualization-Graphviz/)
- **图片处理（Photoshop)**:批处理与动作
- **Blog generator(hexo)**:优化内容页脚模版、修复google/baidu site xml 问题、修复https兼容问题

```bash
vi /theme/yilia/layout/_partial/article.ejs
```
#### 编辑管理工具

- 扩展 Markdown 源文件编辑工具集（nodejs-based）

前期需求：

1）源文件持续修改中出现的回归编辑工作，互相引用的链接较多
2）兼容现有的 Blog Generator (Hexo) 便于融合使用
3）支持 command-line 模式便于调用

后续目标：
1）专题模板一次编辑，多处插入
2）主体自动聚合，类似维基（Template talk）

```js
var fs = require('fs-extra');
var path = require('path');

// All paths are relative to package.json.
var pagesPath = './source/_posts';
var copyFolders = ['./images', './css', './js'];
var outputPath = './tmp';

// First delete everything in the tmp directory.
console.log('Cleaning previous tmp...');
try {
  for (var file of fs.readdirSync(outputPath)){
    fs.removeSync(path.join(outputPath, file));
  }
}
catch (err){
  console.log('Error during cleanup: '+err);
  process.exit(1);
}

// Then read everything in the pages directories.
var pages = {}, pagesMeta = {};

console.log('Loading pages...');
try {
  for(var page of fs.readdirSync(pagesPath)){
    pages[page] = fs.readFileSync(path.join(pagesPath,page),'utf8');
  }
}
catch (err){
  console.log('Error during page loading: '+err);
  process.exit(1);
}

// Generate each page from the data provided, using the template.
console.log('Generating pages...');
try {
  for(var page of Object.entries(pages)) {
    var pageFullName = page[0];
    var pageName = page[0].slice(0, page[0].lastIndexOf('.'));
    var metaData = pagesMeta.hasOwnProperty(pageName+'.json')

    var source_file = pagesPath +'/'+ pageFullName;
    var target_file = outputPath +'/'+ pageFullName;

    //console.log(pageFullName);
    if(source_file.lastIndexOf('.md')>0){

    var source_str = 'http:\\/\\/riboseyim.github.io';
    var target_str = 'https:\\/\\/riboseyim.github.io';

    var exec = require('child_process').exec;
    var cmdStr = 'gsed -e "s/'+source_str+'/'+target_str+'/g" ' + source_file + ' > ' + target_file;
    console.log(cmdStr);

    exec(cmdStr, function(err,stdout,stderr){
      if(err) {
         console.log('exec error:'+stderr);
      } else {
         //console.log(stdout);
      }
      });
    }
  }
}catch (err){
  console.log('Error during page generation: '+err);
  process.exit(1);
}

console.log('--------------Done!--------------------');
```

### v3:201706
![v3:201706](http://riboseyim-qiniu.riboseyim.com/Writer_Tools_Chain_3.png)

#### 更新内容

**OmniFouse**: 计划管理、进度提醒
**数据容灾**：统一使用坚果云
**摄影处理**：图片像素、大小处理
**OmniGraffle**：存量数据合并整理，空间布局／样式配色技巧升级
**图床**：云存储分库、图片命名、批处理
**Evernote、Ulysses**：一般性创意素材从Evernote迁移到Ulysses
**Atom**: 发布定稿前版本采用Atom作为编辑器
**GitBook**: 《Linux Perf Master》 反馈极好
**版权骑士**: 效果不佳，remove

### v2:201701

![v2:201701](http://riboseyim-qiniu.riboseyim.com/Writer_Tools_Chain_2.png)

[分享：从Evernote到Ulysses](http://www.jianshu.com/p/7c453ce42150)

#### 更新内容

**摄影器材**：微单。效果较好的场景：园林、博物馆、航展
**OmniGraffle**：高级应用技能：图层、统一样式、配色技巧
**图床**：采用七牛云
**Evernote**：及时检阅、分类、删除剪辑内容
**GitBook**: 新手入门
**Blog generator(hexo)**: 优化Markdown-->Html自动生成、发布流程

### v1:201606

![v1:201606](http://riboseyim-qiniu.riboseyim.com/Writer_Tools_Chain_1.png)

[分享：思维利器OmniGraffle](http://www.jianshu.com/p/ccc8d64c7202)

#### 更新内容：

**Evernote**：素材仓库
支持所有手机、平板和电脑。在任意一台设备打开Evernote，随时记录一切、轻松收集资料、一键演示笔记、高效协作共享。

**MindManager**：框架梳理
一般人的大部分思考过程都是杂乱无序的，没有逻辑的，最后也没法形成有效的沉淀，更无法找到清晰的结论。不是所有的人都是天生就有很好的逻辑的，MindManager可以辅助进行思维整理、分析、可视化的工具。比如写这篇的时候，就是现在MindManager梳理了一个概要，之后导出为文本作为底稿。

**OmniGraffle**：思维可视化
由The Omni Group制作的一款绘图软件，它曾获得苹果设计奖。可以支持流程图、逻辑图、模型设计等，堪称万能绘图神器。这年头大家都挺忙的，能用一张图表达的意图，就不用写一大堆字啦。

**Markdown**：一次编写，到处发表
Markdown标记语言，我其实很久以前就掌握了，但是使用频率很低，也谈不上什么美感。真正推动我把Markdown纳入个人工具箱的也是写作，可以说是相辅相成吧。它最大的意义在于通过极简的形式，解决了写作成果的移植通用性的问题。

**Ulysses**：样式精美、版本管理、ZoomIn/ZoomOut

**版权骑士**：打击盗版，人人有责。

“维权骑士”有一套自己的监测系统。签约作者用发表的文章，都会纳入“维权骑士”的监测系统，并与各公众号、网站发布内容进行“比对”。一旦发现抄袭，将负责代表作者维护版权。

## 扩展阅读
- [最佳写作实践：从Evernote到Ulysses](https://riboseyim.github.io/2016/06/11/Writing/)
- [技术团队中的作家](https://riboseyim.com/2018/12/02/Writing-Technical/)
- [我的写作工具链](https://riboseyim.com/2017/06/03/Writing-WriterToolChain/)
- [Kanban 看板管理实践精要](https://riboseyim.github.io/2017/08/06/TeamWork-Kanban/)
- [数据可视化（一）思维利器 OmniGraffle 绘图指南 ](https://riboseyim.github.io/2017/09/15/Visualization-OmniGraffle/)

## 参考文献
- [利用 Atom 为自己打造一个专属 Markdown 编辑器](https://zhuanlan.zhihu.com/p/32309940)
- [如何在 hexo 中支持 Mathjax？](https://www.jianshu.com/p/e8d433a2c5b7)
- [使用Atom打造无懈可击的Markdown编辑器](https://www.cnblogs.com/libin-1/p/6638165.html)
- [参考文献管理最佳实践](https://blog.seisman.info/manage-references/)
