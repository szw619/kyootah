---
title: BFC理解与应用
catalog: true
date: 2021-09-08 14:21:05
subtitle:
header-img:
tags:
- BFC
- 边距重合
- 边距塌陷
- css
---

### 定意
>在解释`BFC`之前，先说一下文档流。我们常说的文档流其实分为定位流、浮动流和普通流三种。而普通流其实就是指`BFC`中的`FC`。`FC`是`formatting context`的首字母缩写，直译过来是格式化上下文，它是页面中的一块渲染区域，有一套渲染规则，决定了其子元素如何布局，以及和其他元素之间的关系和作用。常见的FC有BFC、IFC，还有GFC和FFC。`BFC`是`block formatting context`，也就是块级格式化上下文，是用于布局块级盒子的一块渲染区域

简单来说`BFC`是一个独立的区域，它内部的元素都依照它的规则渲染，不会与 BFC 外部打交道。

### 如何触发
* `float`:不为none 
* `overflow`:hidden | scroll | auto; （不是visible） 
* `display`:inline-block | table-cell | table-caption | flex | grid ;（ 非none 非inline 非block） 
* `position`: absolute | fiexed ;（ 非relative） 
可以简单理解成`OFDP`。

### BFC布局规则 
* 1.浮动的元素会被父级计算高度（父级触发了BFC）
* 2.非浮动元素不会覆盖浮动元素位置（非浮动元素触发了BFC）
* 3.margin不会传递给父级（父级触发了BFC）
* 4.两个相邻元素上下margin会重叠（给其中一个元素增加一个父级，然后让他的父级触发BFC）

### 可以解决什么问题

#### 浮动元素父级高度塌陷问题
```html
<div class="father"> 
  <div class="child"></div>
</div>  
```
```css
.father{
  width:200px;
  background:#CCC; 
  border:4px solid #000;
}
.child{
  width:100px;
  height:100px;
  float:left;
  background:red; 
}
```
![](http://img.kyootah.com/2022/02/11/3bb61c98b5c8d.png)
例如这里我们给定一个空间，将空间内的元素浮动，这时候会发现父级的高度并不会被撑开，这种情况就是我们常说的`高度塌陷`。
我们可以利用`BFC`来解决这个问题，给父级设置一个`overflow:hidden`，触发`BFC`。
```css
.father{
  width:200px;
  background:#CCC; 
  border:4px solid #000;
  overflow:hidden;
}
.child{
  width:100px;
  height:100px;
  float:left;
  background:red; 
}
```
 


![](http://img.kyootah.com/2022/02/11/28e96ce1f9180.png)
这个时候可以看到我们的容器高度被撑开了,遵循BFC布局规则第1条: __计算BFC的高度时，浮动元素也参与计算__



#### 元素被浮动元素覆盖问题
```html
<div class="aside"></div>
<div class="main"></div>
```
```css
.main{
  width:200px;
  height:200px;
  background:#CCC;   
}
.aside{
  width:100px;
  height:100px;
  float:left;
  background:red; 
}
```
 ![](http://img.kyootah.com/2022/02/11/2cdfaf30985e6.png)
 可以看到当`aside`浮动了之后覆盖在了没设置浮动的`main`上。
 __为什么会这样__？因为BFC布局规则规定：__每个元素的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此__。因此，虽然存在浮动的元素`aside`，但main的左边依然会与包含块的左边相接触。
__解决方法：利用`BFC`的规则非浮动元素不会覆盖浮动元素位置（非浮动元素触发了BFC）__,给`main`设置`overflow:hidden`。
__适用场景__： 自适应多栏布局（避免多列布局由于宽度计算四舍五入而自动换行）
 ```css
 .main{
  width:200px;
  height:200px;
  background:#CCC;   
  overflow:hidden;
}
```
![](http://img.kyootah.com/2022/02/11/d51a57d70ee73.png)

#### margin重合问题
```html
<div class="father">
  <div class="child1"></div>
  <div class="child2"></div>
</div>
```

```css
.father{
  width:200px;
  background:#ccc;
}

.child1{
  width:100px;
  height:100px;
  background:red;
  margin-bottom:10px;  
}
.child2{
  width:100px;
  height:100px;
  background:red; 
  margin-top:10px; 
}
```
![](http://img.kyootah.com/2022/02/11/9c78fdbd677fc.png)
例如上面两个`child`，分别设置了`margin-bottom`与`margin-top`10px，但实际效果是两个元素的间隔只有 __10px__ ,而非理想的 __20px__
__解决方法__:给其中某个元素给定一个外层并触发BFC。

```html
<div class="father">
  <div class="child1"></div>
  <div class="wrap">
    <div class="child2"></div>
  </div>
</div>
```
```css
.wrap{
  overflow: hidden;
}
```
可以看到间距恢复正常了
![](http://img.kyootah.com/2022/02/11/c1193823448c7.png)



#### margin塌陷问题
```html
<div class="father">
  <div class="child"></div> 
</div>
```
```css
.father{
  width:200px;
  height:200px;
  background:blue;
  margin:60px;  
}
 
.child{
  width:100px;
  height:100px;
  background:red;
  margin:50px;  
}
```
![](http://img.kyootah.com/2022/02/11/b599137ffb6bb.png)
我们给定一个元素`father`并设置`margin:60px`，并在其中放入子元素`child`，设置` margin:50px`，可以看到水平上面的边距生效了，垂直方向没效果,这就是**外边距塌陷**现象
**什么是margin外边距塌陷？**对于两个嵌套关系（父子关系）的块元素，父元素有上外边距同时子元素也有上外边距，此时父元素会塌陷**较大**的外边距值。
**解决方法**：让父元素变成`BFC`,在`father`中加入`overflow:hidden;`
效果：
![](http://img.kyootah.com/2022/02/11/39a3214151401.png)

 