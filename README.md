### App.vue 引入全局公共样式

引入样式库，放到一个统一目录，如common下
```
uni.css  //官方ui库(uni-app的hello-uni-app中有最新的uni.css,可能需要将/static/uni.ttf也要复制到项目中)
animate.css //css动画库 [animate](https://daneden.github.io/animate.css/)
icon.css //图标库，先创建空的css文件
common.css //公共样式 ，先创建空的css文件
```
设置全局样式
```
1. 页面高度100%, 默认字体28upx, 默认行高1.8
page{
	height: 100%;
	font-size: 28upx;
	line-height: 1.8;
	background: #FFFFFF;
}

2. 图片默认100%宽度
image{ width: 100%;}

3. 设置主色调

/* 主背景颜色（橙色） */	
.main-bg-color{background: #FD6801;}
/* 主点击背景颜色 */
.main-bg-hover-color{background: rgba(253,104,1,0.85);}
/* 主字体颜色（橙色） */
.main-text-color{color: #FD6801;}
/* 主边框颜色 */
.main-border-color{ border-color: #F1F1F1;}

```
### pages.json 全局及底部导航配置

全局配置
```
	"globalStyle": {
		"navigationBarTextStyle": "black",
		"navigationBarTitleText": "仿小米商城",
		"navigationBarBackgroundColor": "#FFFFFF",
		"backgroundColor": "#FFFFFF"
	},

```
底部导航配置
```
	"tabBar":{
		"color":"#B1B1B1",
		"selectedColor":"#Fd6801",
		"borderStyle":"black",
		"backgroundColor":"#FFFFFF",
		"list":[
			{
				"pagePath":"pages/index/index",
				"text":"首页",
				"iconPath":"static/tabbar/index.png",
				"selectedIconPath":"static/tabbar/indexSelected.png"				
			},
			{
				"pagePath":"pages/class/class",
				"text":"分类",
				"iconPath":"static/tabbar/class.png",
				"selectedIconPath":"static/tabbar/classSelected.png"				
			},
			{
				"pagePath":"pages/cart/cart",
				"text":"购物车",
				"iconPath":"static/tabbar/cart.png",
				"selectedIconPath":"static/tabbar/cartSelected.png"
			},
			{
				"pagePath":"pages/my/my",
				"text":"我的",
				"iconPath":"static/tabbar/my.png",
				"selectedIconPath":"static/tabbar/mySelected.png"
			}
		]
		
	}

```


### UI 基础库封装(/common/zcm-main.css)
1. 什么是基础封装库(参考boostrap的css, )
```
颜色：布局：边框：内边距：外边距：字体大小：行高：FLEX布局：
```
example: 在index.vue中使用zcm-main.css,加入如下代码到<template>中: (注意：position-fixed bottom-0 在HBuilder的内置浏览器中不支持，需要用#ifdef H5 在zcm-main.css等来条件编译区别，以便可以在内置浏览器中支持。)
```
	<view class="d-flex bg-white border-top position-fixed bottom-0 left-0 right-0 " style="height: 90upx;">
		<view class="flex-1 d-flex j-center a-center flex-column line-h">
			<view class="iconfont icon-xihuan line-h"></view>
			收藏			
		</view>
		<view class="flex-1 d-flex j-center a-center flex-column line-h">
			<view class="iconfont icon-gouwuche line-h"></view>
			购物车
		</view>
		<view style="flex:2.5;" class="text-white main-bg-color d-flex j-center a-center flex-column line-h" hover-class="main-bg-hover-color">加入购物车</view>
	</view>

```
2. upx:是在uni-app中定义的屏幕的宽度为750upx.等同于微信中的rpx. 因此所有的宽度和长度都要以前为基础进行计算.
3. 
### 首页开发(vue部分)
1. 首页pages.json
```
				"app-plus":{
					"scrollIndicator":"none",
					"titleNView":{
						"searchInput":{
							"align":"left",
							"backgroundColor":"#F7F7F7",
							"borderRadius":"4px",
							"disabled":true,
							"placeholder":"智能积木，越野四驱",
							"placeholderColor":"#CCCCCC"
						},
						"buttons":[
							// 消息
							{
								"color":"#989898",
								"colorPressed":"#FD6801",
								"float":"left",
								"fontSize":"22px",
								"fontSrc":"/static/font/iconfont.ttf",
								"text":"\ue67a"
							},
							// 扫一扫
							{
								"color":"#989898",
								"colorPressed":"#FD6801",
								"float":"right",
								"fontSize":"22px",
								"fontSrc":"/static/font/iconfont.ttf",
								"text":"\ue661"
							}
						]
					}
				}

```
2. 首页轮播Swiper组件开发
```
a. 创建一个组件目录及文件:components/index/swiper-image.vue
b. 在swiper-image.vue组件文件中写出轮播组件的template和script
c.  相关的关系：
	1).index.vue中引入组件（注意swiperImage与文件swiper-image的关系,以及@代表的路径的位置）：import swiperImage from "@/components/index/swiper-image.vue"
	2).index.vue中提供数据: swipers 
	3).index.vue中的template中加载组件和数据

```
3. 图标分类组件
组件的创建方法类似swiper组件，然后在index.vue中作如下引用:
```
<index-nav    :resdata="indexnavs"></index-nav>
```
4. 分割线组件:因为很多页面都使用，将其作为全局组件，放在main.js中引入
```
<divider></divider>
```
5. 三图广告位组件
```
		<view class="d-flex">
			<image src="/static/images/demo/demo1.jpg"
			lazy-load="true"
			style="width:373upx; height: 530upx;border-right: 2upx solid #F5F5F5;"
			></image>
			
			<view class="d-flex flex-column">
				<image src="/static/images/demo/demo2.jpg"
				style="width: 375upx;height: 264upx;border-bottom: 2upx solid #F5F5F5;"></image>

				<image src="/static/images/demo/demo2.jpg"
				style="width: 375upx;height: 264upx;"></image>
			</view>			
		</view>
```
5. card 组件封装：
```
1. 在index.vue中引入组件中变量加":" 与不加的区别是啥？ 例如card.vue中的showHead与:showHead的区别？
2. card中组件放到/components/common/card.vue中。

```
6. 公共列表组件封装
```
1. 公共价格封装: /components/common/price.vue
2、公共列表组件：/components/common-list.vue: 在组件中引入组件，即在公共列表组件中引入了价格封装组件.

```
7. 顶部选项卡:
```
a.scroll-view
b.swiper



```
8. 加载数据
```
a. 模拟从后端获取数据，进行数据

```
9. 上拉加载更多
```
a. index.vue中的loadmore

```
10. 

### 

### NVUE基础快速入门
1. 基础组件解析
```
1). 文字写在text中
2). <list>的子组件只能包括以下四种组件或是fix定位的组件
    <cell>: 用于定义列表中的子列表项，类似于HTML中的ul之于li. weex会对<cell> 进行高效的内存回收以达到更好的性能
	<header>: 当<header>到达屏幕顶部时，吸附在屏幕顶部
	<refresh>: 用于给列表添加下拉刷新的功能
	<loading>: 用于与特性和<refresh>类似，用于给列表添加上拉加载更多的功能
3). 图片要给指宽度

```
2. nvue中的css注意事项
```
1). 单位只支持px, 不支持em, rem, pt, %, upx
2). 宽度问题，高度问题 宽：750px=100% 高：1250px = 100%
3). 默认为flex布局
4). 不能合着写
5). 背景颜色只能用background-color
6). 选择器只支持单类
7). 引入要用<style src="@/common/nvue-common.css" </style>
8). <div>组件默认是display:flex; flex-direction:row
```
3. nvue组件解析二
```
在weex中的list使用<list @loadmore="">无效。

改用专门的loading组件才有效，如：
<loading  @loading="onLoading" :display="loadingShow">
	<text> ... </text>
</loading>

```
4. nvue与vue页面进行通讯(一)
```
1). 在nvue里的js中使用uni.postMessage(data)发送数据通讯，data为JSON格式(键值对的值仅支持string)
2). 在App.vue里使用onUniNViewMessage进行监听，监听到后就用uni.$emit(data)通知页面
3). 在指面vue中的onLoad(){}中使用uni.$on(data, e=>{})中进行捕获处理


```

4.  vue与nvue共享的变量和数据
```
nvue不支持vuex
1). uni.storage vue和nvue页面可以使用相同的uni.storage存储。这个存储是持久化的。比如登陆状态可以保存在这里。
2). globalData 小程序有globalData机制，这套机制在uni-app里也可以使用，全端通用。在App.vue文件定义globalData，通过getApp().globalData获取数据

```
5.  
### 搜索页开发

### 
### 
### 网上相关资源
```
0. Uni App官网：https://uniapp.dcloud.io/
1. 阿里巴巴图标库：https://www.iconfont.cn/
2. css动画库:  https://daneden.github.io/animate.css/
3. 取色值工具：
4. 前框开发框架 bootstrap中文网：https://www.bootcss.com/,https://v4.bootcss.com/docs/4.3/utilities/flex/
utilities/colors
5. photoshop: 用于图片的像素大小获取，修图等...
```
### vscode/hbuilder/pycharm/django
```
1. hbuilder-->vscode: https://www.jianshu.com/p/74c06e649e71
2. pycharm/django -->vscode: https://www.cnblogs.com/dangkai/p/10137793.html

```
### 
### 