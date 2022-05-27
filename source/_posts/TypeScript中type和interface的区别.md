---
title: TypeScript中type和interface的区别
catalog: true
toc_nav_num: true
date: 2022-02-23 20:42:40
subtitle:
header-img:
tags:
- TypeScript
---

> 在TypeScript定意数据类型时我们可以用`type`也可以用`interface`，但它们两者间有什么区别，什么场景用中用哪一个比较好，整理一下加深印象。


## interface和type是什么？
### type(类型别名)
类型别名用来给类型起一个名字，使用`type`创建类型别名，类型别名**可以用来表示基本类型、对象类型、联合类型、元组和交集**。
```ts
type userName = string; //基本类型
type uId = string | number; //联合类型
type arr = number[];

// 对象类型
type Person = {
    id: uId; //可以使用定意类型
    name：userName;
    age: number
}

// 泛型
type Tree<T> = {value: T}

const user: Person = {
    id: "901",
    name: "椿",
    age: 22,
    gender: "女",
    isWebDev: false,
};
const numbers: arr = [1, 8, 9];

```
### interface(接口)
接口是用来命名数据结构(例如对象的一种方式); 雨`type`不用，`interface`只能用来描述对象类型，没办法描述基本类型,写法也不同于type
```ts
interface Person {
    id: uId;
    name: userNmae;
    age: number
}
```

## 相似之处/相同点
### 都可以描述`Object`和`Function`
两者都可以用来描述对象或函数，但语法不同
```ts
// 对象
type Point = {
    x: number
    y: number
}
interface Point {
    x: number
    y: number
}

//函数
type SetPoint = (x:number,y:number) => void;
interface SetPoint {
    (x:number,y:number):void
}

```

### 两者都可以被继承(拓展)
**interface 继承 interface**
interface 通过 extends来继承
```ts
interface Person{
    name:string
}
interface Boy extends Person {age:number}

```
**type 继承 type**
type 通过 `&` 来继承
```ts
type Person={
    name:string
}
type Boy = Person & {age:number}
```

`interface` 与 `type` 之间不互斥，可以互相被继承。
**interface 继承 type**
 ```ts
type Person = {
    name:string
}
interface Boy extends Person {age:number}

```
**type 继承 interface**
type 通过 `&` 来继承
```ts
interface Person{
    name:string
}
type Boy = Person & {age:number}
```



## 不同点

### 声明方式不同
type 必须用 “=” 来进行赋值，二interface是声明
```ts
type Person={
    name:string
}
interface Person{
    name:string
}
```

### 可定意的数据类型不同
* 类型别名 `type` 可以用于**基本类型、联合类型或元组类型定义**，而接口 `interface` 不行 
```ts
type T1 = number;
type T2 = string | number; 
type T3 = number[]
```

* 同名接口 `interface` 会自动合并，而类型别名 `type` 不会 
```ts
//同名接口合并
interface User { name: string };
interface User { age: number };

let user: User = { name: 'leo', age: 18 };
user.name; // 'leo'
user.age;  // 18  

// 同名类型别名会冲突
type User = { name: string };
type User = { age : number }; // Error 
```


## type与interface应该在什么时候去使用？
`type`可以声明任何类型，而`interface`只能声明类和对象，在类和对象上`interface`接口合并特效与书写简洁方面我觉得更好一些，所以建议
- 定意基本类型时，使用 type
- 定意元组类型时，使用 type
- 定意函数类型时，使用 type
- 定意联合类型时，使用 type
- 定意映射类型时，使用 type

- **需要利用接口合并特性的时候，使用 interface**
- **定意对象类型且无需使用type的时候，使用 interface**


