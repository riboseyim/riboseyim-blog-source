---
title: Visualization-Canvas
date: 2017-12-19 14:32:51
tags:
categories: 工程技术
---
## 摘要
**待编辑**

<!--more-->

HTML5 <canvas> 标签用于绘制图像（通过脚本，通常是 JavaScript）。不过，<canvas> 元素本身并没有绘制能力（它仅仅是图形的容器） ，您必须使用脚本来完成实际的绘图任务。

Canvas 的默认大小为300像素×150像素（宽×高，像素的单位是px）。但是，可以使用HTML的高度和宽度属性来自定义Canvas 的尺寸。为了在 Canvas 上绘制图形，我们使用一个JavaScript上下文对象，它能动态创建图像（ creates graphics on the fly）。

```html
<canvas id="canvas"></canvas>
```

```js
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');

ctx.fillStyle = 'green';
ctx.fillRect(10, 10, 100, 100);
```

## 资源
- [An HTML 5 Canvas API :IRIS Worm is a real-time data graphing component](https://github.com/gchq/iris-worm)

## 参考文献
- [HTML 5 Canvas 参考手册](http://www.w3school.com.cn/tags/html_ref_canvas.asp)
