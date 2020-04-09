<template> 
<!-- 组件 -->
	<view class="all-content">
		<view class="swiper-box-con">
		    <uni-swiper-dot :info="info" :current="current" field="content" :mode="mode">
		        <swiper class="swiper-box" :autoplay="autoplay"><!-- @change="change" -->
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
		<view class="uni-caption">
			<view class="uni-item">
				<view class="uni-title">今日运势</view>
			</view>
			<view class="uni-item">
				<view class="uni-column">简单快速，查看每日运势，快来试试吧</view>	
			</view>
		</view>
		
		<form class="" @submit="formSubmit">
			<view class="cu-list">				
				<view class="uni-list-cell">
					<view class="uni-list-cell-left">输入姓名:</view>
					<view class="uni-list-input">
					    <input class="uni-input" name="inputname" placeholder="请输入姓名" maxlength="5"/>
					</view>
				</view>
				<view class="uni-list-cell">
					<view class="uni-list-cell-left">选择性别:</view>
					<view class="uni-list-cell-gender">
					    <radio-group name="checkgender">
					        <label>
					            <radio value="man" /><text>男</text>
					        </label>
					        <label>
					            <radio value="women" /><text>女</text>
					        </label>
					    </radio-group>
					</view>
				</view>				
				<view class="uni-list-cell">
					<view class="uni-list-cell-left">出生日期:</view>
					<view class="uni-list-input">
						<picker mode="date" name="birthday" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
							<view class="uni-input">{{date}}</view>
						</picker>
					</view>
				</view>
				<view class="uni-list-cell">
					<view class="uni-list-cell-left">出生时辰:</view>
					<view class="uni-list-input">
						<picker @change="bindBirthTimeChange" :value="BirthTimeIndex" :range="birthtime" range-key="0">
							<view class="uni-input">{{birthtime[BirthTimeIndex][0]}}</view>
							<input style="display: none;" name="birthtime" :value="birthtime[BirthTimeIndex][0]" />
						</picker>
					</view>
				</view>
				<view class="button-sp-area">
					<button class="button-area" type="primary" form-type="submit" >查看运势</button>
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
			const currentDate = this.getDate({
				format: true
			});
			return {
				autoplay:true,
				info: [{content: '../../static/swiper/swiper-1.jpg'},				     
					{content: '../../static/swiper/swiper-2.jpg'},				   
				    {content: '../../static/swiper/swiper-3.jpg'}],
				current: 3,
				mode: 'default',//提示小点的样式
				//
				datas: [
					{type: '心理测试', icon: '/static/home/icon1.png',pageSrc: '/pages/mental/mentalHealth'},
					{type: '星座分析', icon: '/static/home/icon1.png',pageSrc: '/pages/constellation/constellationAnalysis'},
					{type: '八字运势', icon: '/static/home/icon1.png',pageSrc: '/pages/numerology/numerology'},
					{type: '趣味测评', icon: '/static/home/icon1.png',pageSrc: '/pages/self/selfEvaluation'}
				],
				BirthTimeIndex: 0,
				birthtime: [
					['未知(我不知道)','001'],
					['早子时(00:00-00:59)','002'],
					['丑时(01:00-02:59)','003'],
					['寅时(03:00-04:59)','004'],
					['卯时(05:00-06:59)','005'],
					['辰时(07:00-08:59)','006'],
					['巳时(09:00-10:59)','007'],
				],
				date: currentDate,
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
		computed: {
			startDate() {
				return this.getDate('start');
			},
			endDate() {
				return this.getDate('end');
			},
			
		},
		methods: {
			bindDateChange: function(e) {
				this.date = e.target.value
			},
			bindBirthTimeChange: function(e) {
				this.BirthTimeIndex = e.target.value
			},
			getDate(type) {
				const date = new Date();
			
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
			
				if (type === 'start') {
					year = year;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
			
				return `${year}-${month}-${day}`;
			},
			//表单提交
			formSubmit(e){
				let _this = this;
				let formValue = e.detail.value;
				console.log(formValue)
				if(formValue.inputname <= 0){
					uni.showToast({
						icon: 'none',
						title: '请输入姓名'
					})
					return;
				}
				if(formValue.checkgender <= 0){
					uni.showToast({
						icon: 'none',
						title: '请选择性别'
					})
					return;
				}
				if(formValue.birthday <= 0){
					uni.showToast({
						icon: 'none',
						title: '请选择出生日期'
					})
					return;
				}
				if(formValue.birthtime <= 0){
					uni.showToast({
						icon: 'none',
						title: '请选择出生时辰'
					})
					return;
				}
			},
			openinfo(e){
				var newsid = e.currentTarget.dataset.newsid;
				uni.navigateTo({				
					url:'../info/info?newsid='+newsid
				})
			},
			// change(e) {
			//     this.current = e.detail.current;
			// }
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
		margin-top: 15px;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: space-around;
	}
	.tag {
		display: flex;
		width: 140upx;
		height: 150upx;
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
	/* 说明 */
	.uni-caption{
		height:80px;
		flex-direction: column;
		align-items: center;
		padding: 5px;
	}
	.uni-caption .uni-title{
		font-weight:800;
	}
	/* 表单 */	
	.cu-list{
		margin:0px 30px;
		height:220px;		
		background-image: linear-gradient(#a3aab9, #e5e5e5);
		border-radius: 10px;
		flex-direction: column;
	}
	
	.uni-list-cell{
		margin-top: 8px;
		justify-content: space-around;
	}
	.uni-list-input .uni-input {
		width: 190px;
		height: 20px;
		
	}
	.uni-list-cell .uni-list-cell-gender{
		width: 205px;
	}
	.button-sp-area{
		height: 30px;
		justify-content: center;
		margin-top: 8px;		
	}
	.button-area{
		vertical-align:middle;
		text-align: center;
		line-height: 30px;
		width: 180px;
	}
	
	/* 列表 */
	.uni-padding-wrap{
		height: 240px;	
		width: 100%;
	}		
	.uni-media-list-body{height: auto;}
	.uni-media-list-text-top{line-height:1.6em;}
</style>
