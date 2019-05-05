<!-- 
 -->
<template>
	<view class="top bgColor">
		<view class="bar">
			<image v-if="imgSrc" :src="imgSrc+'icon/fenlei.png'" class="icon type"></image>
			<view class="barContent">
				<view @click="navSelect('record')" :class="{sel:record}">录制</view>
				<view @click="navSelect('music')" :class="{sel:music}">音乐馆</view>
				<view @click="navSelect('ranking')" :class="{sel:ranking}">排行</view>
			</view>
			<image v-if="imgSrc" :src="imgSrc+'icon/jiahao.png'" class="icon add"></image>
		</view>
		<view class="serch bgDeepColor" v-if="!record">
			<input type="text" placeholder="搜索" confirm-type="搜索" @confirm="search" @focus="searchFocus" @blur="()=>{this.result = [];}" v-model="searchKey"
			 placeholder-style="color:rgba(255,255,255,.8)" />
			<image class="icon" v-if="imgSrc" :src="imgSrc+'icon/sousuo5.png'"></image>
			<view class="searchResult" v-if="showSearch">
				<view style="width: 90%;height: 80upx;position: relative;display: flex;display: -webkit-flex;align-items: center;justify-content: flex-end;">
					<icon style="margin-top: 20upx;" type="cancel" size="26" @tap="()=>{this.showSearch = false;}"></icon>
				</view>
				<view class="resultInfo" v-for="(item,index) in result" :key='index' @tap="gotoInfo(item.index)">
					{{item.item}}
				</view>
				<view v-if="result.length<=0" style="width: 90%;height: 80upx;color: #CCCCCC;display: flex;display: -webkit-flex;justify-content: center;align-items: center;">
					没有找到您要的内容哦~
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "top",
		props: {
			// 			prodata: {
			// 				type: Object,
			// 				default: {}
			// 			},
		},
		onLoad(){
			this.searchKey = '';
			this.showSearch = false;
		},
		data() {
			return {
				// 搜索结果列表
				result: [],
				// 是否显示搜索列表
				showSearch: false,
				// 搜索结果总列表
				searchRes: [],
				searchKey: '',
				imgSrc: '../../static/image/',
				record: false,
				music: true,
				ranking: false,
				list: [{
					Name: "你的微笑",
					SrcAudio: "http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/556/66405556.aac",
					Author: 'Shart Foeir',
					Album: "平凡的一天sjb出校门才能农村明细表模板弄明白每年相比你每次下班是们重新报聪明程序包",
					SQ: 0,
					DJ: 1,
					HQ: 0,
					VIP: 1
				}, {
					Name: "英雄联盟群雄齐聚",
					SrcAudio: "http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/414/66335414.aac",
					Author: '毛不易',
					Album: "平凡的一天sjb出校门才能农村明细表模板弄明白每年相比你每次下班是们重新报聪明程序包",
					SQ: 1,
					DJ: 1,
					HQ: 0,
					VIP: 1
				}, {
					Name: "TryEverything",
					SrcAudio: "http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/480/66379480.aac",
					Author: 'Shakira',
					Album: "平凡的一天sjb出校门才能农村明细表模板弄明白每年相比你每次下班是们重新报聪明程序包",
					SQ: 0,
					DJ: 1,
					HQ: 0,
					VIP: 0
				}, {
					Name: "我爱过几个人",
					SrcAudio: "http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/339/65390339.aac",
					Author: '邓伦',
					Album: "平凡的一天sjb出校门才能农村明细表模板弄明白每年相比你每次下班是们重新报聪明程序包",
					SQ: 0,
					DJ: 0,
					HQ: 0,
					VIP: 1
				}, {
					Name: "浪子回头",
					SrcAudio: "http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/645/66725645.aac",
					Author: '辉气*蛋',
					Album: "平凡的一天sjb出校门才能农村明细表模板弄明白每年相比你每次下班是们重新报聪明程序包",
					SQ: 1,
					DJ: 1,
					HQ: 0,
					VIP: 1
				}, {
					Name: "生僻字心动不已",
					SrcAudio: "http://cdnringbd.shoujiduoduo.com/ringres/userv1/a48/359/64750359.aac",
					Author: '未知',
					Album: "平凡的一天sjb出校门才能农村明细表模板弄明白每年相比你每次下班是们重新报聪明程序包",
					SQ: 0,
					DJ: 1,
					HQ: 1,
					VIP: 0
				}]
			};
		},
		methods: {
			// 点击搜索结果播放
			gotoInfo(val){
				// console.log(val);
				uni.navigateTo({
					url: '../info/index?playIndex='+val
				});
			},
			// 搜索输入框聚焦
			searchFocus() {
				this.result = [];
				this.searchRes = [];
				for (let i = 0; i < this.list.length; i++) {
					// console.log(this.list[i].Name);
					let arr = ((this.list[i].Name).split("")).concat(('-' + this.list[i].Author).split(""));
					this.searchRes.push(arr);
				}
				// console.log(this.searchRes);
			},
			// 搜索
			search() {
				let keys = this.searchKey.split("");
				let res = [];
				let resKey = [];
				for (let i = 0; i < keys.length; i++) {
					let t = keys[i];
					for (let j = 0; j < this.searchRes.length; j++) {
						let s = this.searchRes[j];
						for (let k = 0; k < s.length; k++) {
							if (s[k] === t) {
								resKey.push(j);
								res.push(s.join(''));
							}
						}
					}
				}
				resKey = resKey.filter(function(element, index, self) {
					return self.indexOf(element) === index;
				});
				res = res.filter(function(element, index, self) {
					return self.indexOf(element) === index;
				});
				for(let h = 0; h < resKey.length; h++){
					this.result.push({index:resKey[h],item:res[h]});
				}
				// for(let i = 0; i < this.list.length; i++){
				// 	console.log(this.list[i]);
				// 	if(this.list[i].Name.indexOf(this.searchKey) != -1 || this.list[i].Author.indexOf(this.searchKey) != -1){
				// 		
				// 	}
				// }
			},
			navSelect(val) {
				this.record = this.music = this.ranking = false;
				this[val] = true;
				this.$emit('navSelect', val);
				sessionStorage.setItem('nav', val);
			}
		},
		beforeMount() {
			let nav = sessionStorage.getItem('nav');
			this.record = this.music = this.ranking = false;
			this[nav] = true;
		},
		watch: {
			searchKey(val) {
				console.log(val);
				if(val !== ""){
					this.showSearch = true;
				}else{
					this.showSearch = false;	
				}
				// if (this.searchKey.length >= 1) {
				// 	this.showSearch = true;
				// }else{
				// 	this.showSearch = false;
				// }
				this.search();
			},

		}
	}
</script>

<style lang="scss" scoped>
	.top {
		padding: 40upx 20upx;
		width: 100%;
		box-sizing: border-box;
		color: #fff;
		font-size: 32upx;

		.bar {
			display: flex;
			justify-content: space-between;
			line-height: 44upx;

			.icon {
				width: 44upx;
				height: 44upx;
			}

			.barContent {
				display: flex;
				width: 360upx;
				margin: 0 auto;
				justify-content: space-between;

				.sel {
					font-size: 36upx;
					font-weight: 600;
				}
			}
		}

		.serch {
			position: relative;
			margin-top: 32upx;
			padding: 10upx 20upx;
			height: 76upx;
			line-height: 60upx;
			box-sizing: border-box;
			color: rgba(255, 255, 255, 1);

			input {
				padding-left: 60upx;
			}

			.icon {
				position: absolute;
				top: 16upx;
				left: 20upx;
				width: 40upx;
				height: 40upx;
			}

			.searchResult {
				position: absolute;
				top: 100%;
				left: 0;
				width: 100%;
				height: 520upx;
				overflow: auto;
				background: #fff;
				border-bottom-left-radius: 10upx;
				border-bottom-right-radius: 10upx;
				z-index: 999;
				display: flex;
				display: -webkit-flex;
				flex-direction: column;
				align-items: center;
				border: 1px solid #ddd;
				box-sizing: border-box;
				
				.resultInfo{
					text-indent: .8em;
					height: 80upx;
					width: 90%;
					display: flex;
					overflow: hidden;
					display: -webkit-flex;
					align-items: center;
					color: #333;
					font-size: 28upx;
					border-bottom: 1upx solid #ddd;
				}
			}
		}
	}
</style>
