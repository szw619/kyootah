---
title: 探索JS原型到原型链
catalog: true 
date: 2021-09-12 21:23:51
subtitle:
header-img:
tags:
- JavaScript深入
- 原型链
---

>原型链，老生常谈的一个概念了，是时候总结巩固一下！

### 相关概念
- **构造函数** ：通过  new 函数名来实例化对象的函数叫构造函数。任何的函数都可以作为构造函数存在。之所以有构造函数与普通函数之分，主要从功能上进行区别的，构造函数的主要 功能为 初始化对象，特点是和new 一起使用。new就是在创建对象，从无到有，构造函数就是在为初始化的对象添加属性和方法。构造函数定义时首字母大写（规范）。
- **prototype** ：每个函数都会有这个属性，这里强调，是函数，普通对象是没有这个属性的（这里为什么说普通对象呢，因为JS里面，一切皆为对象，所以这里的普通对象不包括函数对象）。它是构造函数的原型对象；
- **__proto__** ：每个对象都有这个属性,，这里强调，是对象，同样，因为函数也是对象，所以函数也有这个属性。它指向构造函数的原型对象；
- **constructor** ：这是原型对象上的一个指向构造函数的属性，**函数才有这个属性**


### 构造函数
我们先使用构造函数创建一个对象：
```js
function Person(){

}
var person = new Person();
person.name='Tah'
console.log(person.name)//Tah
```
在这个例子中，Person 就是一个**构造函数**，我们使用 new 创建了一个**实例对象** person。

### prototype
```js
function Person(){

}
Person.prototype.name='Tah';
var person1 = new Person();
var person2 = new Person();
console.log(person1.name)//Tah
console.log(person2.name)//Tah
Person.prototype.name='Ami';
console.log(person1.name)//Ami
console.log(person2.name)//Ami
```

每个函数都有一个 `prototype` 属性。

**什么是原型？**原型就是指`prototype`,JavaScript对象在创建的时候会关联另外一个对象，这个对象就是我们所说的原型。

函数的 `prototype` 属性指向了一个对象，**这个对象是调用该构造函数而创建的实例的原型**，也就是这个例子中的 person1 和 person2 的原型。

每一个实例对象会从原型“继承”属性，就如上面例子重person1与person2都继承了name属性，并且原型上的属性修改的同时实例对象中继承的属性也会跟着修改。


我们来用图表示一下构造函数、实例原型与实例对象之间的关系。
![](https://img.kyootah.com/2022/04/25/530aae411c492.png)
根据上图我们可以看到`Person.prototype`表示`person`对象的原型，而person原型的指向是通过`__proto__`来表示的。

### __proto__
这是每一个JavaScript对象(除了 null )都具有的一个属性，叫`__proto__`，在上图中已经画了出来他们之间的关系，这个属性会指向该对象的原型。
我们来通过一个例子证明这一点：
```js
function Person(){

} 
var person = new Person(); 
console.log(person.__proto__ === Person.prototype)//true 
```


### constructor
每个原型都有一个 constructor 属性指向关联的构造函数。
```js
function Person() {

}
console.log(Person === Person.prototype.constructor); // true
```
通过上面例子我们我们可以来更新一下关系图
![](https://img.kyootah.com/2022/04/26/06a34527e4713.png)
我们来通过代码验证一下上图的关系
```js
function Person() {

}
var person= new Person()
console.log(person.__proto__ == Person.prototype) // true
console.log(Person.prototype.constructor == Person) // true
// 顺便学习一个ES5的方法,可以获得对象的原型
console.log(Object.getPrototypeOf(person) === Person.prototype) // true
```

接着我们更深入地了解一下构造函数、实例原型、和实例之间的关系：
### 实例与原型
**当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。**
我们来举个例子：
```js
function Person() {

}

Person.prototype.name = 'zewen';

var person = new Person();

person.name = 'Kana';
console.log(person.name) // Kana

delete person.name;
console.log(person.name) // zewen
```
这里我们给实例对象`person`设置了一个name值zewen，当我们打印它的时候，自然输出的就是zewen
但是当我们删除person的name值的时候，并不会找不到name这个属性，**而是到person的`__proto__`里面,也就是`Person.prototype`中找name**，所以输出了Kana。



**注意：**
```js
function Person() {

}
var person = new Person();
console.log(person.constructor === Person); // true
```
上面这个例子的解释：当获取 person.constructor 时，其实 person 中并没有 constructor 属性,当不能读取到constructor 属性时，会从 person 的原型也就是 Person.prototype 中读取，正好原型中有该属性，所以：
```js
person.constructor === Person.prototype.constructor
```

### 原型的原型
在前面，我们已经讲了原型也是一个对象，既然是对象，我们就可以找一下它的`__proto__`：
```js
function Person() {

} 
Object.prototype.name = 'zewen'; 
var person = new Person(); 
console.log(person.name) // zewen
console.log(Person.prototype.__proto__===Object.prototype)//true
```
我们根据上面例子来更新一下关系图：
![](https://img.kyootah.com/2022/04/26/6ad3b5c4fdb46.png)


### 原型链
上面说了Person原型(Person.prototype)的`__proto__`是Object.prototype,那Object.prototype的`__proto__`呢
```js
console.log(Object.prototype.__proto__ === null)
```
**Object.prototype.__proto__**也就是最顶端，就是null
我们来更新一下关系图：**图中由相互关联的原型组成的链状结构就是原型链，也就是红色那条线**
![](https://img.kyootah.com/2022/04/27/5b999f8b9f6bd.png)

### 数据类型声明构造函数
在验证过程中发现一个有意思的问题
```js
console.log(Function.__proto__ === Function.prototype) // true 
```
Function的`__proto__`竟然等于他自己的原型
接着试试其他数据类型
```js
console.log(Function.__proto__ === Function.prototype) // true 
console.log(Function.__proto__ === Object.__proto__) // true 
console.log(Object.__proto__ === Array.__proto__) // true 
console.log(Array.__proto__ === Date.__proto__) // true 
console.log(Date.__proto__ === String.__proto__) // true 
function Person() {};
console.log(Person.__proto__ === Function.prototype) // true
console.log(Function.prototype.__proto__ === Object.prototype) // true
```
得出结论：**所有构造函数的__proto__都指向Function.prototype**
我们来用关系图描述一下：
![](https://img.kyootah.com/2022/04/27/50537ec8c58d5.png)

