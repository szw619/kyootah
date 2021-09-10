---
title: 理解JS中的new及其工作
catalog: true
date: 2020-06-30 16:16:40
subtitle:
header-img:
tags:
- 回归JS基础
---

> new到底做了哪些事情？

### new到底做了哪些事情

先举个栗子
```js
function Person(firtName, lastName) {
  this.firtName = firtName;
  this.lastName = lastName;
}

Person.prototype.getFullName = function () {
  return `${this.firtName} ${this.lastName}`;
};

const wyf = new Person('Wu', 'yifang');
console.log(wyf); 
```
查看一下控制台打印的wyf实例
![](http://upload.dreamgotrue.cn/2021/07/30/02f6089556c4f.png)