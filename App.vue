<script>
	export default {
		onLaunch: function () {
			console.log('App Launch')
		},
		onShow: function () {
			console.log('App Show')
		},
		onHide: function () {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	@import './static/css/common';
	/*每个页面公共css */
	uni-page-body{
		width: 100%;
		height: 100%;
	}
</style>
