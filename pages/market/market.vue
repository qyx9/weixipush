<template>
	<view class="market">
		<view class="banner">
			<image src="../../static/banner.png" mode="widthFix"></image>
		</view>
		<view class="decscmenu">
			<scroll-view scroll-x  class="scrollmenu">
				<view class="scrollview" v-for="(item,index) in descmenu" :key="index" @click="menuclicks(index)" 
				:class="Tabindex==index?'actives':''">{{item.name}}</view>
			</scroll-view>
		</view>
		market
		<button type="warn" open-type="share">888</button>
		<view @click="jing">12132132</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				descmenu:[
					{name:"时实热销榜"},
					{name:"时实收益榜"},
					{name:"今日销售榜"}
				],
				Tabindex:0
			}
		},
		onLoad() {
           
		},
		methods: {
               menuclicks(i){
				   this.Tabindex=i;
			   },
			   jiang(){
				   uni.share({
						provider: "weixin",
						scene: "WXSceneSession",
						type: 0,
						href: "http://uniapp.dcloud.io/",
						title: "uni-app分享",
						summary: "我正在使用HBuilderX开发uni-app，赶紧跟我一起来体验！",
						imageUrl: "https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/uni@2x.png",
						success: function (res) {
							console.log("success:" + JSON.stringify(res));
						},
						fail: function (err) {
							console.log("fail:" + JSON.stringify(err));
						}
					});
			   }
		}
	}
</script>

<style>
	.banner{
		width: 100%;
	}
	.banner>image{
		width: 100%;
	}
	.decscmenu{
		color:#ACACAC;
	}
	
	.scrollview{
		font-size: 36upx;
	}
</style>
