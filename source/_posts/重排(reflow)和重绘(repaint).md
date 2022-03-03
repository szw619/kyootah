---
title: 回流(reflow)和重绘(repaint)
catalog: true
date: 2021-08-12 20:16:40
subtitle:
header-img:
tags:
- 渲染队列
- 回流重绘
- 前端进阶
- 项目优化
---

> 平时聊到关于前端优化的问题的时候经常听到`回流重绘`这个词，很多人都知道要减少浏览器的回流和重绘但对于具体操作及其原理比不了解，今天我们就要彻底掌握他！

## 网页生成过程
在了解回流重绘前我们先得知道html的渲染过程。
1. HTML被HTML解析器解析成DOM 树
2. css则被css解析器解析成CSSOM 树
3. 结合DOM树和CSSOM树，生成一棵渲染树(Render Tree)
4. 生成布局（`flow`），即将所有渲染树的所有节点进行平面合成
5. 将布局绘制（`paint`）在屏幕上
**第四步和第五步是最耗时的部分**，这两步合起来，就是我们通常所说的`渲染`。


## 渲染
**网页生成的时候，至少会渲染一次。**

**在用户访问的过程中，还会不断重新渲染**

重新渲染需要重复之前的第四步(重新生成布局)+第五步(重新绘制)或者只有第五个步(重新绘制)。

**重排比重绘大：**
大，在这个语境里的意思是：谁能影响谁？

* 重绘：某些元素的外观被改变，例如：元素的填充颜色
* 重排：重新生成布局，重新排列元素。

就如上面的概念一样，单单改变元素的外观，肯定不会引起网页重新生成布局，但当浏览器完成重排之后，将会重新绘制受到此次重排影响的部分。 
**回流必将引起重绘，重绘不一定会引起回流。**
 
## 回流 (Reflow)
当`Render Tree`中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。

**会导致回流的操作：**

* 页面首次渲染
* 浏览器窗口大小发生改变
* 元素尺寸或位置发生改变
* 元素内容变化（文字数量或图片大小等等）
* 元素字体大小变化
* 添加或者删除可见的DOM元素
* 激活CSS伪类（例如：:hover）
* 查询某些属性或调用某些方法
  

**一些常用且会导致回流的属性和方法:**

* clientWidth、clientHeight、clientTop、clientLeft
* offsetWidth、offsetHeight、offsetTop、offsetLeft
* scrollWidth、scrollHeight、scrollTop、scrollLeft
* scrollIntoView()、scrollIntoViewIfNeeded()
* getComputedStyle()
* getBoundingClientRect()
* scrollTo()


## 重绘(repaint)
当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。


## 浏览器的渲染队列
思考以下代码将会触发几次渲染？
```js
div.style.left = '10px';
div.style.top = '10px';
div.style.width = '20px';
div.style.height = '20px';
```
根据我们上文的定义，**这段代码理论上会触发4次重排+重绘**，因为每一次都改变了元素的几何属性，**实际上最后只触发了一次重排**，这都得益于浏览器的`渲染队列机制` ：
当我们修改了元素的几何属性，导致浏览器触发重排或重绘时。它会把该操作放进渲染队列，等到队列中的操作到了**一定的数量或者到了一定的时间间隔时**，浏览器就会批量执行这些操作。

### 强制刷新队列
```js
div.style.left = '10px';
console.log(div.offsetLeft);
div.style.top = '10px';
console.log(div.offsetTop);
div.style.width = '20px';
console.log(div.offsetWidth);
div.style.height = '20px';
console.log(div.offsetHeight);
```
**这段代码会触发4次重排+重绘**，因为在console中**你请求的这几个样式信息**，无论何时浏览器都会立即执行渲染队列的任务，即使该值与你操作中修改的值没关联。
**因为队列中，可能会有影响到这些值的操作，为了给我们最精确的值，浏览器会立即重排+重绘。**
**我们在开发中，应该谨慎的使用这些style请求，注意上下文关系,避免一行代码一个重排，这对性能是个巨大的消耗**

## 优化建议
### 1.分离读写操作
```js
div.style.left = '10px';
div.style.top = '10px';
div.style.width = '20px';
div.style.height = '20px';
console.log(div.offsetLeft);
console.log(div.offsetTop);
console.log(div.offsetWidth);
console.log(div.offsetHeight);
```
代码注意上下文关系，例如上面这样写的话就只触发了一次重排

### 2.样式集中改变
```js
div.style.left = '10px';
div.style.top = '10px';
div.style.width = '20px';
div.style.height = '20px';
```
虽然现在大部分浏览器有渲染队列优化，不排除有些浏览器以及老版本的浏览器效率仍然低下
建议通过改变class或者csstext属性集中改变样式
```js
// bad
var left = 10;
var top = 10;
el.style.left = left + "px";
el.style.top  = top  + "px";
// good 
el.className += " theclassname";
// good
el.style.cssText += "; left: " + left + "px; top: " + top + "px;";
```

### 3.缓存布局信息
```js
// bad 强制刷新 触发两次重排
div.style.left = div.offsetLeft + 1 + 'px';
div.style.top = div.offsetTop + 1 + 'px';

// good 缓存布局信息 相当于读写分离
var curLeft = div.offsetLeft;
var curTop = div.offsetTop;
div.style.left = curLeft + 1 + 'px';
div.style.top = curTop + 1 + 'px';
```

### 4.离线改变dom
1. 隐藏要操作的dom
在要操作dom之前，通过display隐藏dom，当操作完成之后，才将元素的display属性为可见，因为不可见的元素不会触发重排和重绘。
```js
dom.display = 'none'
// 修改dom样式
dom.display = 'block'
```
2. 通过使用[DocumentFragment](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment)创建一个dom碎片,在它上面批量操作dom，操作完成之后，再添加到文档中，这样只会触发一次重排。
3. 复制节点，在副本上工作，然后替换它


### 5.position属性为absolute或fixed
position属性为absolute或fixed的元素，重排开销比较小，不用考虑它对其他元素的影响(脱离文档流，不会对父元素造成影响)


### 6.CSS相关优化
* **position属性为absolute或fixed**
可以把动画效果应用到position属性为absolute或fixed的元素上，这样对其他元素影响较小.
动画效果还应牺牲一些平滑，来换取速度，这中间的度自己衡量：

比如实现一个动画，以1个像素为单位移动这样最平滑，但是reflow就会过于频繁，大量消耗CPU资源，如果以3个像素为单位移动则会好很多。

* **避免使用table布局**
由于浏览器使用流式布局，对Render Tree的计算通常只需要遍历一次就可以完成，但table及其内部元素除外，他们可能需要多次计算，通常要花3倍于同等元素的时间，这也是为什么要避免使用table布局的原因之一。

* **避免设置多层内联样式**
 
* **避免使用CSS表达式**