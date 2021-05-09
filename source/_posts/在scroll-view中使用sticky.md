---
title: 解决在scroll-view中使用sticky无效
date: 2020-10-16 09:52:26
categories:
- 微信小程序
tags:
- 工作中遇到的问题
# thumbnail:
---

>在小程序的scroll-view使用直接sticky粘性定位会出现无效问题。解决方法：用一个view包住所有子项。

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
