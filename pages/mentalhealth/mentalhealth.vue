<template>	
	<view class="content">
		<view class="111">心理测试</view>
		<view class="222">222</view>
	</view>	
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		onLoad() {
			console.log("页面初始化 执行一次 onLoad")
		},
		onReady() {
			console.log("页面加载完毕 执行一次 onReady")
		},
		onShow() {
			console.log("页面进入执行 执行多次 onShow")
		},
		onHide() {
			console.log("页面离开执行 执行多次次 onHide")
		},
		onPullDownRefresh() {
			console.log("用户执行了下拉动作")
		},
		// 微信小程序分享
		onShareAppMessage() {
			return{
				title:"这条街最靓的仔",
				path:"pages/swiper/swiper",
				imageUrl:"../../static/swiper/swiper-1.jpg"
			}
		}
	}
</script>
	
<style lang="scss">
	.content{display: block;}
</style>
