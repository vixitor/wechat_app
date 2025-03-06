<template>
	<view class="home_header">enigma</view>
	<view>
		<swiper class="home_swiper" indicator-dots="true" autoplay="true" circular="true">
			<swiper-item class="swiper_item">
				<image src="../../static/A1.png"></image>
			</swiper-item>
			<swiper-item class="swiper_item">
				<image src="../../static/B1.png"></image>
			</swiper-item>
			<swiper-item class="swiper_item">
				<image src="../../static/C1.png"></image>
			</swiper-item>
			<swiper-item class="swiper_item">
				<image src="../../static/D1.png"></image>
			</swiper-item>
			<swiper-item class="swiper_item">
				<image src="../../static/E1.png"></image>
			</swiper-item>
		</swiper>
	</view>
	<view class="home_middle_text">选择你喜欢的去探索吧</view>
	<view>
		<scroll-view class="home_middle" scroll-x="true">
			<view class="scroll_object">
				<image class="swiper_image" src="/static/A.png" alt="Image A"></image>
			</view>
			<view class="scroll_object">
				<image class="swiper_image" src="/static/B.png" alt="Image B"></image>
			</view>
			<view class="scroll_object">
				<image class="swiper_image" src="/static/C.png" alt="Image C"></image>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	
	page {
	  background-color: #f4f4f4;
	}
	.swiper_image {
		width: 100%;
		height: 100%;
		object-fit: contain; /* 图片覆盖整个区域 */
		border-radius: 10px; /* 给图片添加圆角 */
	}

	.home_header {          /*抬头样式，有点丑记得改*/
	  background-color: #3498db;
	  color: white;
	  font-size: 20px;
	  text-align: center;
	  padding: 15px 0;
	  margin-bottom: 10px;
	  
	}
	
	.home_swiper {
	  height: 200px;
	}
	
	.swiper_item {         /*滑动板块的样式*/
	  display: -webkit-flex;
	  display: flex;
	  justify-content: center;
	  align-items: center;
	  font-size: 40px;
	  color: white;
	}
	
	.home_middle_text {
	  text-align: center;
	  font-size: 16px;
	  color: #333;
	  margin: 10px 0;
	}
	
	.home_middle {
	  white-space: nowrap;
	  padding: 10px 0;
	}
	
	.scroll_object {   /*下面那个大框的样式*/
	  display: inline-block;
	  background-color: white;
	  border: 1px solid #ccc;
	  border-radius: 10px;
	  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
	  width: 300px;
	  height: 200px;
	  margin-right: 20px;
	  padding: 15px;
	  font-size: 14px;
	  color: #333;
	}
	/* 定义字体样式 */
	.bona-nova-sc-regular {
	  font-family: "Bona Nova SC", serif;
	  font-weight: 400;
	  font-style: normal;
	}
	
	.bona-nova-sc-bold {
	  font-family: "Bona Nova SC", serif;
	  font-weight: 700;
	  font-style: normal;
	}
	
	.bona-nova-sc-regular-italic {
	  font-family: "Bona Nova SC", serif;
	  font-weight: 400;
	  font-style: italic;
	}
	
	.playwrite-mx-guides-regular {
	  font-family: "Playwrite MX Guides", serif;
	  font-weight: 400;
	  font-style: normal;
	}
	
	/* 其他页面样式 */
	.home_header {
	  background-color: #3498db;
	  color: white;
	  font-family: 'Bona Nova SC', serif; /* 设置字体 */
	  font-weight: 400; /* 设置字体粗细 */
	  font-size: 24px;
	  text-align: center;
	  padding: 15px 0;
	  margin-bottom: 10px;
	}
	
	
</style>
