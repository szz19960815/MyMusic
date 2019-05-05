<!-- 数据结构
data:{Name:"这里是名字",SrcAudio:"音频地址",Author:'作者',describe:"这里是描述"",SQ:1,DJ:1,HQ:1,VIP:1}
 -->
<template>
	<view class="playBot borderColorTop">
		<!-- <image class="img zhuan" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1548155771330&di=543688d6a155099dd10e0c5305198b7e&imgtype=0&src=http%3A%2F%2Fimg4.duitang.com%2Fuploads%2Fitem%2F201512%2F20%2F20151220153237_5MSsz.jpeg"></image> -->
		<view class="title">
			<view class="name">{{recordName}}</view>
			<view class="desc">{{recordAuthor}}</view>
		</view>
		<image v-if="!isStart" class="icon" :src="btnImg" @tap="luzhi"></image>
		<view @tap="luzhi" v-if="isStart" style="width: 60upx;height: 60upx;border: 2px solid orangered;border-radius: 100%;box-sizing: border-box;margin-left: 20upx;display: flex;display: -webkit-flex;justify-content: center;align-items: center;">
			<view style="width: 50%;height: 50%;background: orangered;">

			</view>
		</view>
		<!-- <image class="icon liebiao" src="../../static/image/icon/muluH.png"></image> -->
	</view>
</template>

<script>
	const recorderManager = uni.getRecorderManager();
	const innerAudioContext = uni.createInnerAudioContext();

	innerAudioContext.autoplay = true;

	export default {
		props: {},
		data() {
			return {
				btnImg: '../../static/image/icon/kaishiH.png',
				recordName: '未命名',
				recordAuthor: '未知',
				isStart: false,
				voiceList: [],
				voicePath: '',
				isPlay: false,
			};
		},
		onLoad() {
			let that = this;
			recorderManager.onStop(function(res) {
				console.log('recorder stop' + JSON.stringify(res));
				that.$emit('endRecord',res.tempFilePath);
				// that.voicePath = res.tempFilePath;
			});

		},
		methods: {
			// 点击录制按钮
			luzhi() {
				// if(this.btnImg === '../../static/image/icon/kaishiH.png'){
				// 	this.btnImg === '../../static/image/icon/zantingH.png';
				// 	this.start();
				// }
				// if(this.btnImg === '../../static/image/icon/zantingH.png'){
				// 	this.btnImg === '../../static/image/icon/kaishiH.png';
				// 	this.pause();
				// }
				if (this.isStart) {
					this.isStart = false;
					this.end();
				} else {
					this.isStart = true;
					this.start();
				}
			},
			// 开始录音
			start() {
				console.log("开始录音");
				recorderManager.start();
			},
			// 暂停录音
			pause() {
				console.log('暂停录音');
				recorderManager.pause();
			},
			// 继续录音
			resume() {
				console.log("继续录音");
				recorderManager.resume();
			},
			// 结束录音
			end() {
				console.log('结束录音');
				recorderManager.stop();
			},
			// 播放录音
			playVoice() {
				console.log("播放录音");
				if (this.voicePath) {
					innerAudioContext.src = this.voicePath;
					innerAudioContext.play();
				}
			},
			// 暂停播放
			pauseVoice(){
				console.log('暂停播放');
				innerAudioContext.pause();
			}
		}
	}
</script>

<style lang="scss">
	.playBot {
		position: fixed;
		display: flex;
		align-items: center;
		bottom: 0;
		left: 0;
		width: 100vw;
		height: 120upx;
		padding: 20upx 25upx 10upx;
		box-sizing: border-box;
		background: white;

		.img {
			display: block;
			width: 80upx;
			height: 80upx;
			border-radius: 50%;

			&.zhuan {
				-webkit-transform: rotate(360deg);
				animation: rotation 8s linear infinite;
			}
		}

		.title {
			flex: 1;
			padding-left: 30upx;
			font-size: 34upx;

			.desc {
				font-size: 26upx;
				color: rgb(75, 75, 75);
			}
		}

		.icon {
			width: 65upx;
			height: 65upx;
			padding-left: 30upx;

			&.liebiao {
				width: 40upx;
				height: 40upx;

			}
		}
	}
</style>
