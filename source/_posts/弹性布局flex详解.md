---
title: 弹性布局flex详解
subtitle: ""
header-img: "/img/article_header/article_header.png"
catalog: true
toc_nav_num: true
date: 2020-05-11 21:17:04
updateDate: 2020-05-11 21:17:04
categories:
- css
tags:
- css
- flex弹性布局
toc: true
# top: 10

 
---
 

## 背景
Flex是`Flexible Box`的缩写，意为”**弹性布局**”，用来为盒状模型提供最大的灵活性,旨在提供一个更有效地布局、对齐方式，并且能够使容器中的子元素大小未知或动态变化情况下仍然能够分配好子元素之间的空间。

Flex 布局的主要思想是使父容器能够调节子元素的宽度/高度（和排列顺序），从而能够最好地填充可用空间 **（主要是为了适应所有类型的显示设备和屏幕尺寸）** flex布容器能够放大子元素使之尽可能填充可用空间，也可以收缩子元素使之不溢出。

最重要的是，**flexbox布局与方向无关**，不同于常规布局（基于垂直的块（block）和基于水平的内联（inline））。 虽然传统布局适用于页面，但它们对于大型或复杂的应用程序布局来说缺乏灵活性（特别是在改变方向，调整大小，拉伸，收缩等方面）。

注:
* **Flexbox布局最适合应用程序的组件和小规模布局，而 Gird 布局则适用于较大规模的布局。**
* **设为Flex布局以后，子元素的float、clear和vertical-align属性将失效。**

<!-- more -->

## 基本概念
采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。
![基本概念](http://upload.dreamgotrue.cn/2021/04/10/0426548eccf4c.png)
  


## 属性

 ![属性](http://upload.dreamgotrue.cn/2021/04/10/2af8fcd984ece.png)

### display:flex;

![](http://upload.dreamgotrue.cn/2021/04/14/95163b9178a2a.png) 

### flex-direction
设置轴的方向。
* row（默认）：主轴水平方向，起点左端
* row-reverse：主轴水平方向，起点右端
* column：主轴垂直方向，起点上方
* column-reverse：主轴垂直方向，起点下方
  
![](http://upload.dreamgotrue.cn/2021/04/14/6f1554a56176c.png)
![](http://upload.dreamgotrue.cn/2021/04/14/74686ba65f227.png) 

 
### flex-wrap
* nowrap（默认）：不换行
* wrap：换行，第一行在上方
* wrap-reverse：换行，第一行在下方

![](http://upload.dreamgotrue.cn/2021/04/14/07cbf0032f8d3.png)


### justify-content
* flex-start（默认值）：左对齐
* flex-end：右对齐
* center：居中
* space-between：两端对齐，中间间隔平分
* space-around：每个项目间隔相等排列

![](http://upload.dreamgotrue.cn/2021/04/14/b1ceacb101b05.png)
![](http://upload.dreamgotrue.cn/2021/04/14/7fc8fd8191def.png)

### align-items（单轴线）
* stretch（默认）：如果项目没设高度或者高度auto，将占满容器高度
* flex-start：交叉轴的起点对齐
* flex-end：交叉轴的终点对齐
* center：交叉轴的中位点对齐
* baseline：项目的第一页文字的基线对齐

![](http://upload.dreamgotrue.cn/2021/04/14/76e873c7bfcaf.png)

对于 `align-items: stretch` 来说，必须将每一个矩形子元素(flex项)的 **高度设置为 auto**，否则 height 属性将会覆盖该 stretch，如下图
![](http://upload.dreamgotrue.cn/2021/04/15/45cc6510a98cf.png)

对于 `align-items: baseline` 来说，对齐方式基于 **第一行文本内容高度**,要注意如果去掉段落标签或者没内容，矩形子元素(flex项)就会按照每个矩形的底部对齐,如下图：
![](http://upload.dreamgotrue.cn/2021/04/15/b6d6c708b75d3.png)

### align-content（多轴线）
align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
* stretch（默认）
* flex-start
* flex-end
* center
* space-between：各轴线容器两端对齐，中间间隔均等分
* space-around：轴线再容器间隙均等分（两边会有间隙）

![](http://upload.dreamgotrue.cn/2021/04/15/4acd07e1cf3eb.png)


## 项目属性
项目属性用来设置容器内项目（某个元素）的相关样式，用于设置项目的尺寸、位置、对齐方式
基本语法：
* order
* flex-basis
* flex-grow
* flex-shrink
* flex
* align-self

### order
`order` 属性定义项目的排列顺序。数值越小，排列越靠前，默认为0
```html
<div class="flexBox box">
    <div style="order:1">1</div>
    <div style="order:0">0</div>
    <div style="order:2">2</div>
    <div style="order:4">4</div>
    <div style="order:3">3</div>
    <div style="order:-1">-1</div>
</div>
```
![](http://upload.dreamgotrue.cn/2021/04/22/7b053849d3a5d.png)


### Flex-Basis
`flex-basis` 控制一个子元素(flex项)的默认大小，但是它可以被其他的 Flexbox 属性影响（稍后详细介绍）。

下图可以看出它与`width`的作用相同，都能设置宽度，当两者同时存在时Flex-Basis会覆盖width
![](http://upload.dreamgotrue.cn/2021/04/23/3b7197ac49190.png)
![](http://upload.dreamgotrue.cn/2021/04/23/c1498ad79fc38.png)

但是他们之间还是有不同的？flex-basis 对应于 flex 轴线而言的：
![](http://upload.dreamgotrue.cn/2021/04/23/5c881cfec67e9.png)
flex-basis 影响元素在主轴(main axis)上的大小。
 

### flex-grow（拉伸）
属性定义项目的放大比例，默认为0，不放大。

