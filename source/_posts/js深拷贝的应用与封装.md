---
title: js深拷贝的应用与封装
catalog: true
toc_nav_num: true
date: 2021-12-23 21:42:40
subtitle:
header-img:
tags:
- 工作中遇到的问题
- js进阶
- 深拷贝
---

> 在前端开发中，经常会遇到要对一个对象进行拷贝深拷贝，对于深拷贝我们往往只用JSON.parse(JSON.stringify())来进行深拷贝，但对于为什么需要深拷贝，JSON.parse(JSON.stringify())会有什么不足，可能我们并没有去过多了解，现在我们来详细聊聊。'


## 基础知识
在了解深拷贝之前，我们需要了解一些js的基础知识。
### js的数据类型
在`ES6`中，js一共有7种数据类型
* Undefined
* Null
* String
* Number
* Boolean
* Symbol
* Object(包含Array、Function、Date、RegExp、Error)

前面6种属于`简单数据类型（基本数据类型）`，而Object属于`复杂数据类型(引用数据类型)`，数据类型的不同，在内存中存储的方式也不同
**基本数据类型：**因为基本数据类型占用空间小、大小固定，通过按值来访问，属于被频繁使用的数据，存储在栈空间中；
**引用数据类型：**存储在堆空间中，因为引用数据类型占据空间大、大小不固定。 如果存储在栈中，将会影响程序运行的性能，**所以引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址**；
也正是以为存储的方式不同，所以才会出现深拷贝与浅拷贝。

## JSON.stringify
### 实现深拷贝
对于Object的深拷贝，最简单的方法就是JSON.stringify()
```js
var obj={
	name:'zewen',
    age:18
};
var obj1=JSON.parse(JSON.stringify(obj));
```
在日常开发中，大多数的Object的深拷贝可以通过这种方式来简单实现，但他也有他的缺陷。
比如下面这段代码
```js
var obj={
	name:'zewen',
    birthday:new Date('1998/08/22'),
    run:function(){
        console.log('run')
    }
};
var obj1=JSON.parse(JSON.stringify(obj));
console.log(obj1);//{name: "zewen",birthday: "1998-08-21T16:00:00.000Z"}
```
是不是发现出了问题，run方法丢失了，birthday的格式也变了

### JSON.stringify深拷贝的弊端
1. 如果Object中存在`时间对象`，会调用**toJSON(同Date.toISOString)**方法将时间对象转换为**字符串**格式；
2. 如果Object里有`函数`、`undefined`，则序列化的结果会把函数， undefined**丢失**；
3. 如果Object里有`RegExp`、`Error`对象，则序列化的结果将**只得到空对象**；
4. 如果Object里有`NaN`、`Infinity`和`-Infinity`，则序列化的结果会变成**null**；
5. JSON.stringify()只能序列化对象的可枚举的自有属性。 如果Object中的对象是由`构造函数`生成的， 则使用JSON.parse(JSON.stringify(obj))深拷贝后，**会丢弃对象的constructor**；
6. 如果对象中存在`循环引用`的情况也无法正确实现深拷贝；

## 通用深拷贝函数的封装

### 获取数据类型函数
在克隆之前我们先封装一个获取数据类型的方法以便我们针对不同的数据类型，采用不同的拷贝方式
```js
export const getObjType = (obj) => {
  var toString = Object.prototype.toString;
  var map = {
    '[object Boolean]': 'boolean',
    '[object Number]': 'number',
    '[object String]': 'string',
    '[object Function]': 'function',
    '[object Array]': 'array',
    '[object Date]': 'date',
    '[object RegExp]': 'regExp',
    '[object Undefined]': 'undefined',
    '[object Null]': 'null',
    '[object Object]': 'object'
  };
  if (obj instanceof Element) {
    return 'element';
  }
  return map[toString.call(obj)];

 
};

```


### 深拷贝函数
```js
/**
 * 对象深拷贝
 */
export const deepClone = (data) => {
  var type = getObjType(data);//获取数据类型
  var obj;
  if (type === 'array') {
    obj = [];
  } else if (type === 'object') {
    obj = {};
  } else {
    //不再具有下一层次
    return data;
  }
  if (type === 'array') {
    for (var i = 0, len = data.length; i < len; i++) {
      obj.push(deepClone(data[i]));
    }
  } else if (type === 'object') {
    for (var key in data) {
      obj[key] = deepClone(data[key]); 
    }
  }
  return obj;
};

 
```



### 循环引用情况补充

循环引用平时基本不会遇到，毕竟循环引用比较容易出现问题，日常开发中都尽量避免。
在总结跑用例的时候发现之前封装的方法对于循环引用拷贝的情况会出现栈内存溢出的情况，这里补充修改一下方法；
我们先用上面的方法来跑一下测试用例
```js
var field={
    field1: 1,
    field2: undefined,
    field3: {
        child: 'child'
    },
    field4: [2, 4, 8]
};
field.field = field
console.log(deepClone(field));//报错 Uncaught RangeError: Maximum call stack size exceeded
```
可以发现确实进入死循环栈内存溢出了，我们来优化一下
解决思路：利用map，将拷贝过的数据存储一下，如果再遇到，直接取
```js
/**
 * 对象深拷贝，考虑循环引用
 */
export const deepClone = (data,map = new WeakMap()) => {
  var type = getObjType(data);//获取数据类型
  var obj;
  if (type === 'array') {
    obj = [];
  } else if (type === 'object') {
    obj = {};
  } else {
    //不再具有下一层次
    return data;
  }
  if (type === 'array') { 
    for (var i = 0, len = data.length; i < len; i++) {
      obj.push(deepClone(data[i]));
    }
  } else if (type === 'object') { 
    if (map.get(data)) {
        return map.get(data);
    }
    map.set(data, data); 
    for (var key in data) {
      obj[key] = deepClone(data[key],map); 
    }
  }
  return obj;
};

 
```




