<template>
    <view class="swiper-box-con">
        <uni-swiper-dot :info="info" :current="current" field="content" :mode="mode">
            <swiper class="swiper-box" @change="change" :autoplay="autoplay">
                <swiper-item v-for="(item ,index) in info" :key="index">
                    <image class="imgs" :src="item.content" ></image>
                </swiper-item>
            </swiper>
        </uni-swiper-dot>
    </view>
</template>
  
<script>
    import uniSwiperDot from '@/components/uni-swiper-dot/uni-swiper-dot.vue'
    export default {
        components: {
            uniSwiperDot
        },
        data() {
            return {
                autoplay:true,
                info: [{
                    content: 'https://img-cdn-qiniu.dcloud.net.cn/static/images/ad/uniapp-xiaomi.png'
                }, {
                    content: 'https://img-cdn-qiniu.dcloud.net.cn/static/images/ad/uniapp-douyin.jpg'
                }, {
                    content: 'https://img.php.cn/upload/article/000/000/020/5ceb57480713f714.jpg'
                }],
                current: 3,
                mode: 'default',//提示小点的样式
            }
        },
        methods: {
            change(e) {
				console.log(e)
                this.current = e.detail.current;
            }
        }
    }
</script>
 
<style>
    .swiper-box {
        border-radius: 0rpx;
        padding: 0px;
		width: 100%;
		text-align: center;
    }
	.swiper-box-con{
		height: 100;
		width: 100%;
	}
</style>