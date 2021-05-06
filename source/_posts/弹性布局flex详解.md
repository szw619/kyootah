---
title: 弹性布局flex详解
subtitle: "flex学习详细记录，理解各个属性的特性及用法。"
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
> 采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。
![基本概念](http://upload.dreamgotrue.cn/2021/04/10/0426548eccf4c.png)
  


## flex容器属性

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
`flex-basis` 控制一个子元素(flex项)的默认大小，但是它可以被其他的 Flexbox 属性影响（默认值auto）。

下图可以看出它与`width`的作用相同，都能设置宽度，当两者同时存在时Flex-Basis会覆盖width
![](http://upload.dreamgotrue.cn/2021/04/23/3b7197ac49190.png)
![](http://upload.dreamgotrue.cn/2021/04/23/c1498ad79fc38.png)

但是他们之间还是有不同的？flex-basis 对应于 flex 轴线而言的：
![](http://upload.dreamgotrue.cn/2021/04/23/5c881cfec67e9.png)
flex-basis 影响元素在主轴(main axis)上的大小。
`flex-direction`改变为`colunm`则从width切换到影响height
 

### flex-grow（拉伸）
属性定义项目的放大比例，默认为0，不放大。
注：**flex-grow是一个相对值，拉伸放大的区域大小取决于容器承载元素后剩下的区域大小，并根据所设置flex-grow的占比进行比例拉伸**。
示例：
我们先将所有矩形元素（flex项）设置为相同的width,100px,与设置间距(margin)10px，容器宽度大小为1100px:
![](http://upload.dreamgotrue.cn/2021/04/26/5ce176a9f6c4a.png)
现在我们把所有正方形的`flex-grow`设置为1，默认为0（有剩余地方也不做拉伸填充）,
可以看到方块均匀地拉伸并把剩下的空间填充完整（间隙是因为设置了10px的margin）
![](http://upload.dreamgotrue.cn/2021/05/06/b35341e49128e.png)

现在我们把第一个正方形的`flex-grow`设置为2,
这个时候第一个方块的宽度为266.68px，拉伸了`166.68px`,其他方块的的宽度为183.34px,拉伸了`83.34px`,可以看到拉伸的大小为其他方块的2倍，这里可以得出方块总flex-grow数量为2+1+1+1=7,第一块占2/7,其余方块占1/7
![](http://upload.dreamgotrue.cn/2021/05/06/a223eb133e6b0.png)

总结：每个子元素的`flex-grow`都是按比例拉伸的（默认为0，不拉伸），
同`flex-basis`一样`flex-grow`也是只作用与主轴的默认水平方向影响的只有`width`，除非改变`flex-direction`的值。

### Flex Shrink（收缩）
与`flex-grow`同理但相反，`flex-shrink`设置的是当容器主轴长度不足以承载所有项目且`flex-wrap`没有设置换行的情况下，项目对应的收缩比例。
同`flex-grow`一样，是相对值，`flex-shrink`默认值是1，所以他们允许被收缩。
例：我们先把容器宽度设置500px，每个方块宽高设置100px
![](http://upload.dreamgotrue.cn/2021/05/06/caa72ce543167.png)
我们把容器宽度从500px缩小到300px，可以看到各个项目均等比例缩小了。
![](http://upload.dreamgotrue.cn/2021/05/06/a4d53f5e50f56.png)
这个时候我们把第二个方块的`flex-shrink`设置为**0**,不收缩，可以看到方块2就不会进行收缩固定了原来的宽度
![](http://upload.dreamgotrue.cn/2021/05/06/43fdc7dd47c8d.png)
这个时候有的小伙伴就要问，如果全部都设置为0呢？可以从下图看到，全部设置不收缩的情况下容器就会被撑开了。
![](http://upload.dreamgotrue.cn/2021/05/06/965f7894183a7.png)
现在我们来研究一下收缩的大小问题。
`flex-shrink`收缩跟`flex-grow`一样是根据项目`flex-shrink`设置的总比例来收缩的。

我们试下把第一个方块`flex-shrink`设置为2，第二块设置为3。
![](http://upload.dreamgotrue.cn/2021/05/06/7d80718c1f77b.png)
最终得到第一块宽度50px,第二块宽度25px,其余75px,对应第二块跟第二块的收缩大小是其他方块的一倍跟两倍，这个是怎么来的呢？让我们列个表格计算一下。
![](http://upload.dreamgotrue.cn/2021/05/06/7638b10032503.png)
根据表格可以看出：当容器宽度为300px时，容器内项目总宽度500px，超出的200px分别按比例在各个方块内扣除了，方块一收缩了200px的2/8,方块二3/8,其余1/8。
这个时候我们设置一下把第一个方块设置`flex-shrink`为100
可以看出方块1不见了，width变成了0
![](http://upload.dreamgotrue.cn/2021/05/06/1a11daac4434e.png)
这里可以看出当收缩比例占比超出项目宽度的时候，项目的主轴长度会变成0，然后其余项目按比例收缩剩余的大小。这里200px分给了项目1还剩150px,然后项目2对应收缩了150px的3/6，其他项目收缩1/6就可以得出上图结果了。

总结：`flex-shrink`设置的是当项目总宽度大于容器宽度且没设置换行时项目的收缩比例，一样也是只作用与主轴的默认水平方向影响的只有`width`，除非改变`flex-direction`的值。

### flex
`flex` 是 `flex-grow`，`flex-shrink` 和 `flex-basis` 的缩写。
默认值是**flex:0 1 auto** 即：flex-grow:0;flex-shrink:1;flex-basis:auto;
