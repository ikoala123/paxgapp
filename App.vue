<script>
	export default {
		//初始化完成时触发（全局只触发一次）
		onLaunch: function() {
			console.log('App Launch')
		},
		// 启动，或从后台进入前台显示
		onShow: function() {
			console.log('App Show')
		},
		//监听离开程序 从前台进入后台
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */	
	/* uni.css - 通用组件、模板样式库，可以当作一套ui库应用 */
	@import './common/uni.css';
	page,
	view {
		display: flex;
	}
</style>
