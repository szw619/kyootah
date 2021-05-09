---
title: div横向排版的几种方式
catalog: true
toc_nav_num: true
date: 2020-04-12 21:30:45
categories:
- css
tags:
- css
toc: true
---

div盒子本身默认样式属性是独占一行，平时将各个内容横向排版是最常见的操作，记录总结一下div的几种横向排列方法~

<!--more-->

以下面代码为例，列举下横向div排列的方法

```
<div class="wrap">
    <div class="item div1">div1</div>
    <div class="item div2">div2</div>
    <div class="item div3">div3</div>
</div>

<style>
.wrap{
  width:500px;
  background:#f0f0f0;
}
.wrap .item{
  width:100px;
  height:100px;
  background:#ccc;
  text-align:center;
  line-height:100px;
  margin:10px;
}
</style>
```
![image](http://upload.dreamgotrue.cn/2021/04/07/e52a3bb76424c.png)

## 一、浮动

```css
.div1{
    float:left;
}
.div2{
    float:left;
}
.div3{
    float:right;
}
```
效果：
![image](http://upload.dreamgotrue.cn/2021/04/07/5dc5bf37973d5.png)

float 的特点：
* 左右浮动同时用时，顺序就会出现颠倒
* 不会撑开，可以看到wrap的背景颜色没了，height变成0
* 字会环绕在浮动元素周围


## 二、inline-block 行块标签

```css
.div1, .div2, .div3{
    display: inline-block;
}
```
![image](http://upload.dreamgotrue.cn/2021/04/07/eeafe59460bbe.png)
inline-block存在的小问题：

1. 用了display:inline-block后，存在间隙问题，间隙为4像素，这个问题产生的原因是换行引起的，因为我们写标签时通常会在标签结束符后顺手打个回车，而回车会产生回车符，回车符相当于空白符，通常情况下，多个连续的空白符会合并成一个空白符，而产生“空白间隙”的真正原因就是这个让我们并不怎么注意的空白符。
  
2. 去除空隙的方法:对父元素添加，{font-size:0}，即将字体大小设为0，那么那个空白符也变成0px，从而消除空隙
 

 ## 三、flex 弹性盒子

```css
.wrap{
    display:flex;
}
```
![image](http://upload.dreamgotrue.cn/2021/04/10/446ba728945a7.png)

 可以通过 `justify-content` 属性调整子元素的水平对齐方式：

* `flex-start`	默认值。项目位于容器的开头。	
* `flex-end`	项目位于容器的结尾。	 
* `center`	项目位于容器的中心。	 
* `space-between`	项目位于各行之间留有空白的容器内。	 
* `space-around`	项目位于各行之前、之间、之后都留有空白的容器内。	 
* `initial`	设置该属性为它的默认值。请参阅 initial。 
* `inherit`	从父元素继承该属性
