<template>
	<view class="recommend">
		<swiper :autoplay="true" :interval="3000" :duration="1000" circular>
			<swiper-item v-for="(item,index) in bannnerimgs" :key="index" class="bannerimg">
				<image :src="item.image_url" mode="widthFix"></image>
			</swiper-item>
		</swiper>
		<view class="shop">
			<image src="../../static/shop2.png" mode="widthFix"></image>
		</view>
		<view class="relist">
			<view class="relistFirst" @click="testdata">
				<text>测试入口</text>
			</view>
			<view class="relistTi">
				<view class="market"></view>
				<view class="market"></view>
			</view>
		</view>
		<view class="bg"></view>
		<view class="decscmenu">
			<scroll-view scroll-x id="tabmenu" class="scrollmenu">
				<view class="scrollview" v-for="(item,index) in descmenu" :key="index" @click="menuclick(index)" 
				:class="Tabindex==index?'actives':''">{{item.name}}</view>
			</scroll-view>
		</view>
		<view class="porduct">
			<view class="productList" v-for="(item,index) in product" :key="index">
				<image src="../../static/product.png" mode="widthFix"></image>
				<view class="msg">台湾脆梨</view>
			</view>
		</view>	
		<navigator url="../login/login">登录页面</navigator>
		今日推荐
	</view>
</template>

<script>
	export default{
		data(){
			return{
				descmenu:[
					{name:"综合"},
		            {name:"价格"},
					{name:"销量"}
				],
				Tabindex:0,
				product:[1,2,3,4,5],
				bannnerimgs:''
			}
		},
		methods:{
			menuclick(i){
				this.Tabindex=i;
			},
			bannerimg(){
				uni.request({
					url: 'http://appserver.wujie520.cn/thirdreturn/index/themelist?page=1',
					method: 'GET',
					data: {},
					success: res => {
						console.log(res.data.theme_list_get_response.theme_list)
						this.bannnerimgs=res.data.theme_list_get_response.theme_list
					}
				});
			},
			
			testdata(){
				uni.navigateTo({
					url: '../productmess/productmess?id=3762335246',
				});
			}
		},
		// #ifdef H5
		onShow() {
			this.bannerimg()
		},
		// #endif
	   onLoad:function(e){
		   console.log(e);
	   	   this.bannerimg();
	   }
	}
</script>

<style>
	/* bannerimg */
	.recommend .bannerimg>image{
		width: 100%;
	}
	.relist{
		box-sizing: border-box;
		width: 100%;
		height: 384upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		border-top:2upx #EEEEEE solid;
	}
	.relistFirst{
		width: 50%;
		border-right: 2upx #EEEEEE solid;
		border-bottom: 2upx #EEEEEE solid;
	}
	.relistTi{
		width: 50%;
		height: 382upx;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}
	.market{
		height: 50%;
		border-bottom: 2upx #EEEEEE solid;
	}
	.bg{
		width: 100%;
		height: 28upx;
		background-color: #EEEEEE;
	}
	/*  */
	.productList{
		width: 100%;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.productList>image{
		width: 28%;
		height: 160upx;
	}
	.msg{
		width: 72%;
		height: 160upx;
	}
</style>
