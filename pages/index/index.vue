<template> 
<!-- 组件 -->
	<view class="all-content">
		<view class="swiper-box-con">
		    <uni-swiper-dot :info="info" :current="current" field="content" :mode="mode">
		        <swiper class="swiper-box" @change="change" :autoplay="autoplay">
		            <swiper-item v-for="(item ,index) in info" :key="index">
		                <image class="imgs" :src="item.content" ></image>
		            </swiper-item>
		        </swiper>
		    </uni-swiper-dot>
		</view>
		
		<view class="tags">
			<block v-for="(value, index) in datas" :key="index" >
				<view class="tag" @click="goPage(value.pageSrc)">
					<image class="tag-img" :src="value.icon"></image>
					<text class="tag-text">{{value.type}}</text>
				</view>
			</block>
		</view>
		
		<form class="form-action">
			<view class="uni-form">
				<view class="uni-form-item uni-title">今日运势</view>
				<view class="uni-form-item uni-column">简单快速，查看每日运势，快来试试吧</view>				
				<view class="uni-form-item name-gender">
					<view class="uni-form-sub uni-name">姓名：
					        <input class="uni-input" name="input" placeholder="请输入姓名" />
					</view>
					<view class="uni-form-sub uni-gender">性别：
					    <radio-group name="radio">
					        <label>
					            <radio value="radio1" /><text>男</text>
					        </label>
					        <label>
					            <radio value="radio2" /><text>女</text>
					        </label>
					    </radio-group>
					</view>
				</view>
				
				<view class="uni-list-cell">
					<view class="uni-list-cell-left">出生日期</view>
					<view class="uni-list-cell-db">
						<picker mode="date" name="deliverytime" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
							<view class="uni-input">{{date}}</view>
						</picker>
					</view>
				</view>
				<view class="uni-list-cell">
					<view class="uni-list-cell-left">出生时辰</view>
					<view class="uni-list-cell-db">
						<picker @change="bindDeliverTimeChange" :value="DeliverTimeIndex" :range="delivertime" range-key="0">
							<view class="uni-input">{{delivertime[DeliverTimeIndex][0]}}</view>
							<input style="display: none;" name="day" :value="delivertime[DeliverTimeIndex][1]" />
						</picker>
					</view>
				</view>
				
			</view>
		</form>
			
		<view class="content">
			<view class="uni-list">
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in news" :key="index" @tap="openinfo" :data-newsid="item.post_id">
					<view class="uni-media-list">
						<image class="uni-media-list-logo" :src="item.author_avatar"></image>
						<view class="uni-media-list-body">
							<view class="uni-media-list-text-top">{{item.title}}</view>
							<view class="uni-media-list-text-bottom uni-ellipsis">{{item.created_at}}</view>
						</view>
					</view>
				</view>
			</view>		
		</view>
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
				info: [{content: '../../static/swiper/swiper-1.jpg'},				     
					{content: '../../static/swiper/swiper-2.jpg'},				   
				    {content: '../../static/swiper/swiper-3.jpg'}],
				current: 3,
				mode: 'default',//提示小点的样式
				//
				datas: [
					{type: '123', icon: '/static/home/icon1.png',pageSrc: '/pages/mental/mentalHealth'},
					{type: '123', icon: '/static/home/icon1.png',pageSrc: '/pages/constellation/constellationAnalysis'},
					{type: '123', icon: '/static/home/icon1.png',pageSrc: '/pages/numerology/numerology'},
					{type: '123', icon: '/static/home/icon1.png',pageSrc: '/pages/self/selfEvaluation'}
				],
				DeliverTimeIndex: 0,
				delivertime: [
					['未知','001'],
					['早子时(00:00-00:59)','002'],
					['丑时(01:00-02:59)','003'],
					['寅时(03:00-04:59)','004'],
					['卯时(05:00-06:59)','005'],
					['辰时(07:00-08:59)','006'],
				],
				
				news:[],
			}
		},
		onLoad() {
			uni.showLoading({
				title:"加载中..."
			})
			uni.request({
				url:'https://unidemo.dcloud.net.cn/api/news',
				method:'GET',
				data:{},
				success:res => {
					this.news = res.data;
					uni.hideLoading();
				},
				fail: () => {},
				complete: () => {}
			})
		},
		methods: {
			bindDateChange: function(e) {
				console.log(e)
				this.date = e.target.value
			},
			bindDeliverTimeChange: function(e) {
				this.DeliverTimeIndex = e.target.value
			},
			
			openinfo(e){
				var newsid = e.currentTarget.dataset.newsid;
				uni.navigateTo({				
					url:'../info/info?newsid='+newsid
				})
			},
			change(e) {
				console.log(e)
			    this.current = e.detail.current;
			}
		}
	}
</script>

<style>
	/* 整体 */
	.all-content{
		flex-direction:column;
	}
	/* 轮播图 */
	.swiper-box-con{
		height: 150px;		
	}
	.swiper-box {	
		padding: 5px;
		width: 100%;
		height: 150px;
	}
	.imgs{
		width: 100%;
		height: 150px;
	}
	/* 中间导航 */
	.tags {
		margin-top: 20upx;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: space-around;
	}
	.tag {
		display: flex;
		width: 140upx;
		height: 180upx;
		box-sizing: border-box;
		text-align: center;
		flex-direction: column;
		align-items: center;
		margin-bottom: 6px;
	}
	.tag-img {
		width: 110upx;
		height: 110upx;
		margin-bottom: 12upx;
	}
	.tag-text {
		font-size: 24upx;
		color: #555555;
	}

	/* 表单 */	
	.uni-form{
		flex-direction: column;
		margin: 5%;
		height:250px;		
		background-image: linear-gradient(#999999, #e5e5e5);
		border-radius: 10px;		
	}
	
	
	/* 列表 */
	.uni-padding-wrap{
		height: 240px;	
		width: 100%;
	}		
	.uni-media-list-body{height: auto;}
	.uni-media-list-text-top{line-height:1.6em;}
</style>
