---
title: 解决在scroll-view中使用sticky无效
date: 2020-10-16 09:52:26
tags:
- 微信小程序
- 踩坑
- scroll-view
- sticky
# cover:
---

记录一下微信小程序的一个坑，在scroll-view使用sticky无效，解决方法：用一个view包住所有子项。

<!-- more -->

```css
.container{
  width: 100%;
  height: 800rpx;
}
.header {
  position: sticky;
  position: -webkit-sticky;
  top: 0;
  width: 100%;
  height: 120rpx;
  background-color: black;
}
.content {
  width: 100%;
  height: 3000rpx;
  background-color: red;
} 
```
```xml
<scroll-view class="container">
  <view>
    <view class="header"></view>  
    <view class="content"></view>
  </view>
</scroll-view>
```
