<template>
	<view class="info" :style="bgStyle">
		<!-- '' -->
		<view class="bg"></view>
		<view class="content">
			<view class="top" @tap="()=>{this.showComment=false;}">
				<view class="name">{{audioList[audioPlaySrc].name}}</view>
				<view class="author">{{audioList[audioPlaySrc].author}}</view>
				<view class="bar">
					<view>标准</view>
					<view>音效</view>
					<view>视频</view>
				</view>
			</view>
			<view class="micImg">
				<image :class="playState?'zhuan':''" :src="audioList[audioPlaySrc].img" ></image>
			</view>
			<!-- <view class="micWord">Oh oh oh oh oooh</view> -->
			<view class="bot">
				 <view class="lineBar">
				 	<view class="time star">{{nowmiaoForc}}</view>
					<slider class="line" :value="nowmiao" min="0" :max="allmiao" @change="sliderChange" block-size="15" activeColor="#55A532" />
				 	<view class="time end">{{allmiaoForc}}</view>
				 </view>
				 <view class="btn">
				 	<view @tap="audioWayFunc">
						<image v-if="audioWay==0" class="iconbtn" src="../../static/image/icon/xunhuanbofang.png"></image>
						<image v-if="audioWay==2" class="iconbtn" src="../../static/image/icon/danquxunhuan.png"></image>
						<image v-if="audioWay==1" class="iconbtn" src="../../static/image/icon/suijibofang.png"></image>
					</view>
				 	<view class="palybtn">
				 		<view @tap="upPlay"><image class="iconbtn" src="../../static/image/icon/shangyishou.png"></image></view>
				 		<view @tap="play">
							<image v-if="!playState" class="iconbtn play" src="../../static/image/icon/kaishi-2.png"></image>
							<image v-if="playState" class="iconbtn play" src="../../static/image/icon/zanting-2.png"></image>
						</view>
				 		<view @tap="nextPlay"><image class="iconbtn" src="../../static/image/icon/xiayishou.png"></image></view>
				 	</view>
				 	<view><image class="iconbtn" src="../../static/image/icon/yinleliebiao.png"></image></view>
				 </view>
				 <view class="tool">
				 	<view @tap="collectFunc">
						<image v-if="!collect" class="iconbtn" src="../../static/image/icon/shoucang.png"></image>
						<image v-if="collect" class="iconbtn" src="../../static/image/icon/shoucang-2.png"></image>
					</view>
				 	<view><image class="iconbtn" src="../../static/image/icon/xiazai.png"></image></view>
				 	<view><image class="iconbtn" src="../../static/image/icon/share.png"></image></view>
				 	<view><image @tap="open" class="iconbtn" src="../../static/image/icon/liuyanjianyi.png"></image></view>
				 </view>
			</view>
		</view>
		<view class="comment" v-if="showComment">
			<view class="commentTitle">
				<icon class="closeComment" @tap="close" type="clear" size="26"></icon>
			</view>
			<view class="commentList">
				<view class="commentInfo" v-for="(item,index) in commentList" :key="index">
					<view class="touxiang">
						<image :src="item.img" mode=""></image>
					</view>
					<view class="commentBox">
						<view class="name">
							{{item.name}}
						</view>
						<view class="con">
							{{item.comment}}
						</view>
					</view>
				</view>
			</view>
			<view class="commentInput">
				<input type="text" class="input" v-model="sendComment" placeholder="请留下你的精彩评论" />
				<button type="primary" @tap="sendCon" class="send">发送</button>
			</view>
		</view>
	</view>
</template>

<script>
	// 
	let innerAudioContext = ''
	export default {
		onLoad(option) {
			if('playIndex' in option){
				this.audioPlaySrc = Number(option.playIndex)
			}else{
				this.audioPlaySrc = 0;
			}
		},
		props:{
			playIndex:0,
		},
		components:{
		},
		data() {
			return {
				sendComment:'',
				showComment:false,
				nowmiao:0,//当前时间
				allmiao:0,//全部时间
				lineBarWid:520,//进度条的宽度跟css一只
				playState:0,//播放状态
				audioCont:'',
				audioList:[
					{name:'你的微笑',
					 author:'Shart Foeir',
					 src:'http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/556/66405556.aac',
					 img:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1547630730529&di=f37e409de12f9930e25507c3252a462c&imgtype=0&src=http%3A%2F%2Fmp2.iqiyipic.com%2Fimage%2F20180827%2F03%2F41%2Fh_1199242471_h_601_400_400.jpg',
					 },
					{name:'英雄联盟群雄齐聚',
					 author:'毛不易',
					 src:'http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/414/66335414.aac',
					 img:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1547630762297&di=2c3317bf38f107287414b1b0cd33f0e5&imgtype=0&src=http%3A%2F%2Fsingerimg.kugou.com%2Fuploadpic%2Fpass%2Fsofthead%2F400%2F20140829%2F20140829161553861786.jpg',
					 },
					{name:'TryEverything',
					 author:'Shakira',
					 src:'http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/480/66379480.aac',
					 img:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1547471202346&di=bd9b5e27beb9341b058d21e521e8d75e&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201707%2F27%2F20170727210705_zcijE.jpeg',
					 },
					{name:'我爱过几个人',
					 author:'邓论',
					 src:'http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/339/65390339.aac',
					 img:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1547630610810&di=1a0c6d9557aa2bba822c54694a55429e&imgtype=0&src=http%3A%2F%2Fmp0.iqiyipic.com%2Fimage%2F20180827%2Fff%2F79%2Fh_1199249539_h_601_400_400.jpg',
					 },
					 {name:'浪子回头',
					  author:'辉气*蛋',
					  src:'http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/645/66725645.aac',
					  img:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1547631130547&di=076c9275ce160baa2ff5cb66eed4ad53&imgtype=0&src=http%3A%2F%2Fwww.mixtapetorrent.com%2Fsystem%2Ffiles%2Ffollowmernb.jpg',
					  },
					 {name:'生僻字心动不已',
					  author:'〃°ω°〃',
					  src:'http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/359/64750359.aac',
					  img:'https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1547631330468&di=517d6b42ed32567a5798fd9bbe13ff9c&imgtype=0&src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20170830%2F2dbe36dfa0a14c0c94b672f445c02a21.jpeg',
					  },
				],
				commentList:[{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/1.jpeg',
					comment:'哭着吃过饭的人，是能够走下去的',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/2.jpeg',
					comment:'我想做一个能在你葬礼上描述你一生的人',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/3.jpeg',
					comment:'等我嗨死后把我的骨灰撒到海里，不是怕有人在坟头蹦迪，也不为大海更宽广，只是为了我死了也能浪起来',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/sc1.jpg',
					comment:'每个人的裂痕，最后都变成故事的花纹',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/sc2.jpeg',
					comment:'你那么孤独，却说一个人真好',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/sc3.jpeg',
					comment:'当你觉得孤独无助时，想一想还有十几亿的细胞只为了你一个人而活',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/sc4.jpeg',
					comment:'一个人久了，煮个饺子看见两个粘在一起的也要给它分开！',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/sc5.jpg',
					comment:'我在最没有能力的年纪，碰见了最想照顾一生的人。',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/sc7.jpg',
					comment:'世界如此广阔，人类却走进了悲伤的墙角',
				},{
					name:'游客'+Math.floor(Math.random()*99999999999),
					img:'/static/image/sc8.jpg',
					comment:'周杰伦把爱情比喻成龙卷风，我觉得特别贴切。因为很多人，像我，一辈子都没见过龙卷风。',
				}].reverse(),
				audioPlaySrc:0,//当前播放的歌曲index
				audioWay:0,//播放方式 0顺序播放 1随机播放 2单曲循环
				collect:0,//是否收藏
			}
		},
		computed:{
			width:function (){
				return 'width:' + this.nowmiao/this.allmiao * this.lineBarWid + 'upx'
			},
			playWidth:function () {
				return 'transform:translate3d(' + (this.nowmiao / this.allmiao) * this.lineBarWid + 'upx,0,0);'
			},
			nowmiaoForc:function (){
				return this.$pubFuc.secondFormact(this.nowmiao)
			},
			allmiaoForc:function(){
				return this.$pubFuc.secondFormact(this.allmiao)
			},
			bgStyle:function (){
				return 'background-image:url('+this.audioList[this.audioPlaySrc].img+')'
			}
		},
		mounted:function() {
			// this.audioPlaySrc = 0
			this.audioInit();
		},
		methods:{
			// 发送评论
			sendCon(){
				if(this.sendComment.length>=1){
					let config = {
						name:'游客'+Math.floor(Math.random()*99999999999),
						img:'/static/image/sc17.jpg',
						comment:this.sendComment
					};
					this.commentList.unshift(config);
					this.sendComment = '';
					// this.showComment = false;
				}
			},
			// 打开评论
			open(){
				this.showComment = true;
			},
			// 关闭评论
			close(){
				this.showComment = false;
			},
			audioInit(){
				let src = this.audioList[this.audioPlaySrc].src
				if(innerAudioContext){
					innerAudioContext.destroy()
					innerAudioContext = ''
					//销毁====================
				}
				innerAudioContext = uni.createInnerAudioContext();
				innerAudioContext.src = src
				innerAudioContext.autoplay = true
				//获取时长
				let dura = setInterval(()=>{
					this.allmiao = Math.floor(innerAudioContext.duration)
					if(this.allmiao){
						clearInterval(dura)
					}
				})
				//监听事件
				innerAudioContext.onPlay(()=>{
					this.playFunc()
				})
				innerAudioContext.onPause(()=>{
					this.pauseFunc()
				})
				innerAudioContext.onTimeUpdate((e)=>{
					this.nowmiao = Math.floor(innerAudioContext.currentTime)
				})
				innerAudioContext.onEnded(()=>{
					this.nextPlay()
				})
			},
			playFunc(){
				this.playState=1
			},
			pauseFunc(){
				this.playState= 0
			},
			sliderChange(e) {
				this.nowmiao = e.detail.value
				innerAudioContext.seek(this.nowmiao)
			},
			play(){
				if(this.playState){
					//暂停
					innerAudioContext.pause()
				}else{
					//播放
					innerAudioContext.play()
				}
			},
			nextPlay(){
				if(this.audioWay==1){
					//随机
					this.audioPlaySrc = Math.floor(Math.random()*10)%this.audioList.length;
				}else if(this.audioWay==0){
					//顺序
					if(this.audioPlaySrc >= (this.audioList.length-1)){
						this.audioPlaySrc = 0
					}else{
						this.audioPlaySrc = this.audioPlaySrc+1
					}
				}//单曲循环
				this.audioInit()
			},
			upPlay(){
				if(this.audioWay==1){
					//随机
					this.audioPlaySrc = Math.floor(Math.random()*10)%this.audioList.length;
				}else if(this.audioWay==0){
					//顺序
					if(this.audioPlaySrc < 1){
						this.audioPlaySrc = this.audioList.length-1
					}else{
						this.audioPlaySrc = this.audioPlaySrc-1
					}
				}//单曲循环
				this.audioInit()
			},
			audioWayFunc(){
				if(this.audioWay>1){
					this.audioWay = 0
				}else{
					this.audioWay = this.audioWay+1
				}
			},
			collectFunc(){
				this.collect = !this.collect
			},
		},
		destroyed(){
			innerAudioContext.destroy()
			innerAudioContext = ''
		}
	}
</script>

<style lang="scss" scoped>
.info{
	position: fixed;
	right: 0;
	left: 0;
	width:100%;
    height:100%;
	background-position:center;
    background-repeat: no-repeat;
    background-size:cover;
	.bg{
		position: fixed;
		z-index: 0;
		right: 0;
		left: 0;
		height: 100%;
		width: 100%;
		filter: blur(40upx);
		background: inherit;
	}
	.content{
		position: fixed;
		z-index: 1;
		right: 0;
		left: 0;
		height: 100%;
		width: 100%;
		color: #FFFDEF;
		
		.top{
			margin-top: 20upx;
			width:100%;
			text-align:center;
			.name{
				font-size: 46upx;
			}
			.author{
				font-size: 34upx;
				line-height: 60upx;
			}
			.bar{
				width: 330upx;
				justify-content: space-between;
				display: flex;
				margin: 20upx auto;
				view{
					width: 100upx;
					height: 40upx;
					border: solid 1upx #FFFDEF;
					border-radius: 2upx;
					text-align: center;
				}
			}
		}
		.micImg{
			 margin: 25upx auto;
			 image{
				display: block;
				margin: 0 auto;
				width: 600upx;
				height: 600upx;
				border: solid 16upx rgba(0,0,0,.15);
				border-radius:50%;
				&.zhuan{
					-webkit-transform: rotate(360deg);
					animation: rotation 8s linear infinite;
				}
			 }
		}
		.micWord{
			text-align: center;
		}
		.bot{
			position: fixed;
			bottom: 40upx;
			left: 0;
			width: 100%;
			padding: 0 26upx;
			box-sizing: border-box;
			.lineBar{
				display: flex;
				padding: 20upx 0;
				justify-content: space-between;
				.line{
					margin: 0;
					width:520upx;
				}
			}
			.iconbtn{
				display: block;
				width: 50upx;
				height: 50upx;
				margin:0 auto;
			}
			.btn{
				display: flex;
				justify-content: space-between;
				padding: 40upx 15upx;
				.palybtn{
					display: flex;
					width:500upx;
					margin-top:-15upx;
					view{
						flex: 1;
						align-items: center;
					}
					.iconbtn{
						width: 80upx;
						height: 80upx;
						&.play{
							margin-top: -20upx;
							width: 120upx;
							height: 120upx;
						}
					}
					
					
				}
			}
			.tool{
				display: flex;
				view{
					flex: 1;
					text-align: center;
				}
			}
			
		}
	}
	.comment{
		position: fixed;
		bottom: 0%;
		width: 100%;
		height: 80%;
		background: white;
		z-index: 99;
		border-top-left-radius: 20upx;
		border-top-right-radius: 20upx;
		display: flex;
		display: -webkit-flex;
		flex-direction: column;
		align-items: center;
		overflow: hidden;
		
		.commentTitle{
			width: 94%;
			height: 60upx;
			position: relative;
			margin-bottom: 30upx;
			margin-top: 10upx;
			
			.closeComment{
				position: absolute;
				right: 0;
			}
		}
		
		.commentList{
			flex: 1;
			// background: red;
			overflow: auto;
			width: 94%;
			display: flex;
			display: -webkit-flex;
			flex-direction: column;
			
			.commentInfo{
				display: flex;
				display: -webkit-flex;
				// min-height: 140upx;
				margin: 10upx 0;
				
				.touxiang{
					width: 80upx;
					height: 80upx;
					border-radius: 100%;
					margin: 10upx;
					overflow: hidden;
					background: #eee;
					
					image{
						width: 100%;
						height: 100%;
					}
				}
				.commentBox{
					flex: 1;
					display: flex;
					display: -webkit-flex;
					flex-direction: column;
					
					.name{
						width: 100%;
						height: 40upx;
						color: #ccc;
						font-size: 30upx;
						overflow: hidden;
					}
					
					.con{
						font-size: 30upx;
					}
				}
			}
		}
		
		.commentInput{
			height: 120upx;
			// background: yellow;
			width: 94%;
			display: flex;
			display: -webkit-flex;
			align-items: center;
			border-top: 1px solid #ddd;
			
			.input{
				margin-left: 20upx;
				width: 480upx;
				height: 80upx;
				// border: 1px solid black;
				line-height: 96upx;
			}
		}
	}
}
</style>

<style scoped="scoped">
	uni-input div{
		width: 100%;
	}
</style>
