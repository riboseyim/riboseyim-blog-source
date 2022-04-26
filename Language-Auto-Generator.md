---
title: 玩转编程语言:基于Node.js构建自定义代码生成器
date: 2017-12-21 17:11:32
tags: [Nodejs,Developer,DevOps]
categories: 工程技术
---
## 摘要

在真实的软件开发过程中，无论使用何种编程开发语言，都不可避免的会遇到代码重复的问题。如何处理重复的问题，可以选择情怀（手动再敲一遍），也可以选择 Copy-to-Copy ，或者选择代码生成器。正如在之前的文章  [我的写作工具链](https://riboseyim.github.io/2017/06/03/Writing-WriterToolChain/) 中，我介绍过一种 Blog 生成器 hexo ，可以将 Markdown 格式的内容自动生成方便发布的 HTML 格式。本文将还原 hexo 的运行原理，为解决类似问题提供一些参考思路。

>示例：通过 Markdown 文件声明模板（源代码），通过脚本生成 HTML 文件（目标代码），并预览代码生成效果。

<!--more-->

## 一、Hello Node.js

#### Step 1: 准备环境 (dependencies)

开发语言 [Node.js](https://riboseyim.github.io/2017/05/16/Language-Node-lang/), 一个能够运行 JavaScript 的开放源代码、跨平台运行环境。

- npm init — 初始化 root 目录
- npm i -s live-server — 该模块支持本示例生成静态 HTML 站点，提供热部署能力
- npm i -s nodemon — 该模块支持当文件变化自动执行重构任务
- npm i -s concurrently — 该模块支持支持并发执行任务、脚本(scripts/tasks)
- npm i -s markdown-it — 该模块提供 Markdown 文件解析器

```bash
mkdir project-generator
mkdir project-generator/pages
mkdir project-generator/pages_meta
mkdir project-generator/js
mkdir project-generator/css
mkdir project-generator/images
mkdir project-generator/build_scripts
mkdir project-generator/build

cd project-generator
npm init 
npm i -s concurrently
npm i -s fs
npm i -s fs-extra
npm i -s markdown-it
npm i -s live-server 
npm i -s nodemon
```
![](http://riboseyim-qiniu.riboseyim.com/Language-Auto-Generator.png)

####  Step 2: 准备元数据

例如：**pages/index.md**
```
# Home page

Hello world!

[Link to another page](./other.html)

```

例如：**pages_meta/index.json** 用于存储一些需要的元数据（参数、固定内容等），JSON 文件格式方便后面调用。

```json
{
  "lang": "en",
  "title": "Index",
  "stylesheets": ["./css/style.css"],
  "scripts": ["./js/main.js"],
  "charset": "utf-8",
  "description": "This is a page",
  "keywords": "page, sample",
  "author": "None",
  "favicon": "./images/favicon.png",
  "viewport": "width=device-width, initial-scale=1",
  "extra": []
}
```

#### Step 3: 编写模板和构建脚本（template & build Script）

代码生成器中需要定制开发的部分包括 **builder.js** 和 **pages_template.js**。build.js 相当于 main 函数，控制入口和流程，加载资源数据、调用 generator 任务，与 Makefile 和 Ant.xml 非常类似。pages_template.js 依赖的组件是 [markdown-it](https://markdown-it.github.io/markdown-it/#MarkdownIt.configure) ，负责将 Markdown 源文件转换输出成 HTML 文件。builder.js 将 pages_template.js 视为一个模块引用：**pageTemplate.generatePage(pageContent, metaData))** 因此可以根据需要定制多个不同的 XXX_template.js 或者在每个 template.js 中定义其它函数。

**builder.js**

```js
var pageTemplate = require('./page_template');

// All paths are relative to package.json.
var pagesPath = './pages';
var pagesMetaPath = './pages_meta';
var copyFolders = ['./images', './css', './js'];
var outputPath = './build';

// Clean
console.log('Cleaning previous build...');
for (var file of fs.readdirSync(outputPath)){
    fs.removeSync(path.join(outputPath, file));
}

//Loading
console.log('Loading pages metadata...');
  for(var pageMeta of fs.readdirSync(pagesMetaPath)){
    pagesMeta[pageMeta] = fs.readFileSync(path.join(pagesMetaPath,pageMeta),'utf8');
  }
}

// Generate
console.log('Generating pages...');
for(var page of Object.entries(pages)) {
    var pageName = page[0].slice(0, page[0].lastIndexOf('.'));
    var metaData = pagesMeta.hasOwnProperty(pageName+'.json')
      ? JSON.parse(pagesMeta[pageName+'.json'])
      : {};
    metaData.title = metaData.title || pageName;
    var pageContent = page[1];
    fs.writeFileSync(
      path.join(outputPath,pageName+'.html'),
      pageTemplate.generatePage(pageContent, metaData));
  }
}

// Copy
console.log('Copying folders...');
  for(var copyFolder of copyFolders){
    fs.copySync(copyFolder, path.join(outputPath,copyFolder));
  }

```

**pages_template.js**
```js
var md = require('markdown-it')();

module.exports = {
  generatePage: function(pageContent,pageMeta){
    return`<!DOCTYPE html>
<html lang="${pageMeta.lang || this.defaultMeta.lang}">
  <head>
    <title>${pageMeta.title || this.defaultMeta.title}</title>
    <meta charset="${pageMeta.charset || this.defaultMeta.charset}">
    <meta name="description" content="${pageMeta.description || this.defaultMeta.description}">
    <meta name="keywords" content="${pageMeta.keywords || this.defaultMeta.keywords}">
    <meta name="author" content="${pageMeta.author || this.defaultMeta.author}">
</head>
<body>
  ${md.render(pageContent)}
</body>
</html>
    `;
  }
}
```
#### Step 4: 优化任务脚本

在 Step 1 步骤中，npm init 创建了一个文件：**package.json**，我们可以定义其中的 “scripts” , 执行 **npm run start** 将默认在 1080 端口开启 Web 服务。

![](http://riboseyim-qiniu.riboseyim.com/Language-Auto-Generator-HTML.png)

```json
{
  "name": "coffee",
  "version": "1.0.0",
  "description": "beyond my coffee",
  "main": "index.js",
  "scripts": {  
    "build-pages": "node ./build_scripts/builder.js",
    "start": "concurrently --kill-others \"nodemon -e js,json,css,md -i build -x \\\"npm run build-pages\\\"\" \"live-server ./build\""
  },
  "author": "@RiboseYim"
}
```

```js
$ npm run build-pages

> coffee@1.0.0 build-pages /generator-code
> node ./build_scripts/builder.js

Cleaning previous build...
Loading pages...
Loading pages metadata...
Generating pages...
Copying folders...
Done!

$ npm run start

> coffee@1.0.0 start /Users/yanrui/project/generator-code
> concurrently --kill-others "nodemon -e js,json,css,md -i build -x \"npm run build-pages\"" "live-server ./build"

[0] [nodemon] 1.14.1
[0] [nodemon] to restart at any time, enter `rs`
[0] [nodemon] watching: *.*
[0] [nodemon] starting `npm run build-pages`
[1] Serving "./build" at http://127.0.0.1:8080
[1] Ready for changes
[1] GET /js/main.js 404 42.133 ms - 23
[1] GET /js/main.js 404 12.204 ms - 23
[0]
[0] > coffee@1.0.0 build-pages /Users/yanrui/project/generator-code
[0] > node ./build_scripts/builder.js
[0]
[0] Cleaning previous build...
[0] Loading pages...
[0] Loading pages metadata...
[0] Generating pages...
[0] Copying folders...
[0] Done!
[0] [nodemon] clean exit - waiting for changes before restart
[1] Change detected build/index.html
[1] Change detected build/images
```

## Application

### Hexo

#### Hexo Startup

#### Hexo Theme

- Hexo Theme [Cafe](https://github.com/giscafer/hexo-theme-cafe)

#### Hexo Questions

- Hexo g 失败【Uncaught SyntaxError: Invalid or unexpected token】：临时办法-复制到Editplus查看乱码

## 扩展阅读

- [玩转编程语言系列](https://riboseyim.github.io/2017/05/26/Language/)


## 参考文献
- [Building a simple static page generator with Node.js](https://hackernoon.com/building-a-simple-static-page-generator-with-node-js-4f58f680c47d)
