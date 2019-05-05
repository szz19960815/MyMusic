<template>
	<view class="record">
		<!-- 这是录制 -->
		<!-- 录制列表 -->
		<view v-if="list.length<=0" style="color: #CCCCCC;font-size: 26upx;text-align: center;margin-top: 40upx;">暂时还没有录制好的内容哦~</view>
		<view class="recordList" v-if="list.length>=1">
			<view class="recordCard" v-for="(item,index) in list" :key="index">
				<view style="color: #F55500;margin-right: 10upx;font-size: 26upx;margin-left: 10upx;">{{index+1}}</view>
				<view class="recordName">
					<view>{{item.name+(index+1)}}</view>
					<view>{{item.author}}</view>
				</view>
				<image v-if="!item.isPlay" style="width: 60upx;height: 60upx;" src="../../static/image/icon/kaishiH.png" @tap="playVocie(item)"></image>
				<view v-if="item.isPlay" @tap="stopVoice(item)" style="width: 60upx;height: 60upx;border: 2px solid orangered;border-radius: 100%;box-sizing: border-box;margin-left: 20upx;display: flex;display: -webkit-flex;justify-content: center;align-items: center;">
					<view style="width: 50%;height: 50%;background: orangered;">
					</view>
				</view>
			</view>
		</view>
		<play-bot @endRecord="getVoice"></play-bot>
	</view>
</template>

<script>
	import playBot from '../../components/playbot.vue'
	
	const recorderManager = uni.getRecorderManager();
	const innerAudioContext = uni.createInnerAudioContext();
	
	innerAudioContext.autoplay = true;
	innerAudioContext.loop = true;
	console.log(innerAudioContext)
	
	export default {
		name:'record',
		components:{
			playBot,
		},
		onLoad() {
			// let that = this;
			// innerAudioContext.onStop(function(res) {
			// 	for(let i = 0; i < this.list.length; i++){
			// 		this.list[i].isPlay = false;
			// 	}
			// });
		},
		data() {
			return {
				list:[],
			}
		},
		methods: {
			getVoice(val){
				console.log(val)
				this.list.push({name:'未命名',author:'未知',path:val,isPlay:false});
			},
			// 播放录音
			playVocie(obj){
				innerAudioContext.src = obj.path;
				obj.isPlay = true;
				innerAudioContext.play();
				console.log(innerAudioContext);
			},
			// 停止播放
			stopVoice(obj){
				console.log('停止播放');
				innerAudioContext.src = obj.path;
				obj.isPlay = false;
				innerAudioContext.stop();
			}
		}
	}
</script>

<style lang="scss">
.record{
	width: 100%;
	height: 100%;
	overflow: auto;
	display: flex;
	display: -webkit-flex;
	flex-direction: column;
	.recordList{
		flex: 1;
		margin: 0 20upx;
		overflow: hidden;
		display: flex;
		display: -webkit-flex;
		flex-direction: column;
		.recordCard{
			height: 100upx;
			width: 100%;
			display: flex;
			display: -webkit-flex;
			align-items: center;
			border-bottom: 1px solid #ddd;
			.recordName{
				flex: 1;
				display: flex;
				display: -webkit-flex;
				flex-direction: column;
				justify-content: space-around;
			}
		}
	}
}
</style>
