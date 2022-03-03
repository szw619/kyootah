---
title: 理解JS中的new及其工作
catalog: true
date: 2020-06-30 16:16:40
subtitle:
header-img:
tags:
- JS基础
- JavaScript深入
---

> 日常开发中`new`经常会出现在代码里，是时候花点时间深入理解一下`new`相关的知识点了。

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