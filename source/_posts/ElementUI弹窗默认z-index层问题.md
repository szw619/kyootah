---
title: ElementUI弹窗默认z-index层问题级
catalog: true
toc_nav_num: false
toc: false
date: 2020-07-08 17:14:12
subtitle:
header-img:
tags:
- 工作中遇到的问题
---



>当我在Element的dailog组件中使用富文本编辑器tinymce-editor的时候发现图片上传弹窗的层级比Element组件的层级低了不少，导致显示被遮挡的情况，后来发现在引用的时候可以设置默认属性，记录一下加深印象。

dailog组件默认会加上一个z-index
![](http://upload.dreamgotrue.cn/2021/05/08/60f5654be8bc0.png)

官网文档有说明这个问题
```
Vue.use(Element, { size: 'small', zIndex: 3000 });
```
![](http://upload.dreamgotrue.cn/2021/05/08/b74f1f827cda1.png)

