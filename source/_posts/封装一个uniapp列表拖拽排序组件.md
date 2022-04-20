---
title: 封装一个uniapp列表拖拽排序组件
catalog: true
toc_nav_num: true
date: 2021-09-04 21:46:27
categories:
- 微信小程序
tags:
- 工作中遇到的问题
- 拖拽排序
- uniapp
---

### 封装思路
1. 渲染列表使用`absolute`布局，然后拖拽时对各个项目的高度进行动态变更;
2. 利用`longtap`事件，监听开启对某个项目的拖拽状态;
3. 利用`touchmove`事件，对拖拽的高度进行实时监听;
4. 利用`touchend`事件，对原数组进行重排并输出;
 

### 代码实现 
直接贴代码，注释讲解
```html
<template>
	<view class="drag-box" :style="{ height: list.length * itemHeight + 'rpx' }">
		<view
			v-for="(item, index) in dataList"
			:key="index"
			:style="{ top: item.top + 'px', height: itemHeight - 1 + 'rpx' }"
			class="drag-item"
			:class="{ 'drag-active': item.isActive }"
			@longtap="longtap(item)"
			@touchstart="touchstart"
			@touchmove.stop.prevent="touchmove"
			@touchend="touchend(item)"
		>
			<slot :item="item"></slot>
		</view>
	</view>
</template>
```
js
```js
export default {
	props: {
		list: {//项目数组
			type: Array,
			default: () => []
		},
		itemHeight: { //项目高度 为了方便拖拽计算，对列表的每一项使用了固定高度。
			type: [Number],
			default: 70
		}
	},
	data() {
		return {
			activeItem: null,//当前拖拽中的项目
			isDrag: false,
			dragTargetY: 0,//拖拽开始时记录第一次点击高度
			dataList: [],
			sortIndexList: []
		};
	},
	watch: {
		list: {
			immediate: true,
			deep: true,
			handler(list) {
				this.setList(list);
			}
		}
	},
	methods: {
        // 拖拽前记录一下高度
		touchstart(e) {
			console.log('touchstart', e);
			this.dragTargetY = e.touches[0].pageY;//记录点击的屏幕位置
		},
        //长按项目开启拖拽
		longtap(item) {
			this.activeItem = item;
			this.isDrag = true;
			item.isActive = true;
		},

        //拖拽过程event
		touchmove(e) {
			if (!this.isDrag) {
				return;
			}
			let newY = e.touches[0].pageY;
			let d = newY - this.dragTargetY;//计算偏移高度
			this.activeItem.top += d; //对拖拽项目的top进行变更实现跟手效果

			let prevIndex = this.sortIndexList[this.activeItem.index] - 1;//找出上一个项目
			let nextIndex = this.sortIndexList[this.activeItem.index] + 1;//找出下一个项目
			if (prevIndex >= 0 && d < 0) {
				let item = this.getItemByIndex(prevIndex);//
				if (this.activeItem.top < item.top) {//如果拖拽往上的距离超过了上一个项目的top，则进行换位逻辑
					this.swapArray(item);//换位
				}
			} else if (nextIndex < this.list.length && d > 0) {
				let item = this.getItemByIndex(nextIndex);
				if (this.activeItem.top > item.top) {
					this.swapArray(item);
				}
			} 
			this.dragTargetY = newY;
		},

        //拖拽结束，根据每个项目的结果index重排处理数据输出新列表
		touchend(item) {
			if (!this.isDrag) {
				return;
			}
			this.isDrag = false;
			item.isActive = false;
			this.activeItem.top = this.sortIndexList[this.activeItem.index] * this.rowHeight;
			let sortList = [];
			Array(this.dataList.length)
				.fill(0)
				.forEach((v, index) => {
					let tempObj = this.deepClone(this.getItemByIndex(index));
					delete tempObj.isActive;
					delete tempObj.top;
					delete tempObj.index;
					sortList.push(tempObj);
				});
			this.$emit('change', sortList);
		},
		getItemByIndex(index) {
			for (let i = 0; i < this.sortIndexList.length; i++) {
				if (this.sortIndexList[i] === index) {
					return this.dataList[i];
				}
			}
			return null;
		},
        //换位操作
		swapArray(item) { 
			//列表中两个元素交换位置
			let index = this.sortIndexList[this.activeItem.index];
			this.sortIndexList[this.activeItem.index] = this.sortIndexList[item.index];
			this.sortIndexList[item.index] = index;
			item.top = index * this.rowHeight;//对换位的项目高度进行变更实现移动效果
			this.count = 0;
		},

        //初始化数据
		setList(list) {
			this.dataList = list.map((item, index) => {
				this.sortIndexList.push(index);
				return {
					...item,
					isActive: false,
					top: index * this.rowHeight,
					index: index
				};
			});
		},
        //简单深拷贝
		deepClone(obj) {
			let result = {},
				oClass = this.isClass(obj);
			console.log(oClass);
			for (let key in obj) {
				let copy = obj[key];
				if (this.isClass(copy) == 'Object') {
					result[key] = arguments.callee(copy);
				} else if (this.isClass(copy) == 'Array') {
					result[key] = arguments.callee(copy);
				} else {
					result[key] = obj[key];
				}
			}
			return result;
		},
		isClass(o) {
			if (o === null) return 'Null';
			if (o === undefined) return 'Undefined';
			return Object.prototype.toString.call(o).slice(8, -1);
		}
	},
	mounted() {},
	computed: {
		rowHeight() {
			const res = uni.getSystemInfoSync();
			let screenWidth = res.screenWidth;
			if (this.itemHeight) {
				return (this.itemHeight * screenWidth) / 750;
			} else {
				return 0;
			}
		}
	}
};
```
```css
.drag-box {
	width: 100%;
	height: 100%;
	position: relative;
	.drag-item {
		width: 100%;
		text-align: center;
		transition: all 0.5s;
		background-color: #fff;
		z-index: 1;
		border-bottom: 1rpx solid #f5f5f5;
		position: absolute;
	}
}
.drag-active {
	box-shadow: 0 8px 20px 0 #e6e6e6;
	transform: scale(1.1);
	z-index: 9 !important;
	transition: box-shadow 0.5s, transform 0.5s, top 0s !important;
}
```
