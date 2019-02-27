<template>
	<view class="bar">
		<view class="hours">
			<view class="hlist" v-for="(item,index) in hour" :key="index" @click="hourclick(index)"
			:class="tabIndex==index?'hourstyle':''">{{item.name}}</view>
		</view>
		<view class="goodList" v-for="(item,index) in goodList" :key="index" @click="goodLists(item.goods_id)">
		<view class="hproduct">
			<view class="num">
				{{index+1}}
			</view>
			<image :src="item.goods_thumbnail_url" mode="widthFix"></image>
			<view class="hmsg">
			  <text>{{item.goods_name}}</text> 
			  <view class="rooll">
				  <text>卷￥{{item.coupon_discount/100}}</text>
			  </view>
			  <view class="rollprice">
				  <view class="price">
					 <del> <text>拼多多价￥{{item.min_normal_price/100}}</text></del>
					 <text>卷后价￥{{(item.min_normal_price-item.coupon_discount)/100}}</text>
				  </view>
				  <view class="rollmall" @click="rollmall">领{{item.coupon_discount/100}}元卷购买</view>
			  </view>
			</view>
		</view>
		</view>
       bar
	</view>
</template>

<script>
	export default {
		data() {
			return {
				hour:[
					{name:"半小时热销"},
					{name:"2小时热销"},
					{name:"24小时热销"},
				],
				tabIndex:0,
				goodList:[]
			}
		},
		onLoad:function(e) {
			console.log(e);
			uni.showLoading({
				title: '加载中...',
				mask: false
			});
           uni.request({
           	url: 'http://appserver.wujie520.cn/thirdreturn/index/salesvolume?page=1',
           	method: 'GET',
           	data: {},
           	success: res => {
				uni.hideLoading();
				console.log(res.data)
				this.goodList=res.data.goods_search_response.goods_list;
           	}
           });
		},
		methods: {
             hourclick(i){
				 this.tabIndex=i;
			 },
			 goodLists(id){
				 uni.navigateTo({
				 	url: '../productmess/productmess?id='+id
				 });
			 },
			 rollmall(){
				 
			 }
		}
	}
</script>

<style lang="less">
	.bar{
		height: 4000upx;
		padding-top: 140upx;
	}
	.hours{
		box-sizing: border-box;
		margin:20upx 5%;
		width: 90%;
		height: 60upx;
		font-size: 32upx;
		background-color: #FEFEEA;
		border: 2upx solid #FFD350;
		border-radius:30upx;
		color: #FFD559;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		position: fixed;
		top: 0;
	}
	.hlist{
		text-align: center;
		line-height: 60upx;
		box-sizing: border-box;
		width: 33.33333%;
		height: 56upx;
	}
	.hourstyle{
		border-radius:26upx;
		background: linear-gradient(to right,#FFCB00,#FEA513);
		color: #FFFFFF;
	}
	.goodList{
		width: 100%;
		height: 160upx;
		margin-bottom: 80upx;
	}
	.hproduct{
		width: 100%;
		height: 160upx;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
		.num{
			width: 10%;
			text-align:center;
			color: #FFFFFF;
			line-height: 44upx;
			height: 44upx;
			width: 44upx;
			border-radius: 50%;
			background: linear-gradient(#FFD206,#FEA828);
		}
		>image{
			width:30%;
			height: 160upx;
			border-radius: 10upx;
		}
		.hmsg{
			width: 60%;
			display:flex;
			font-size: 24upx;
			flex-direction: column;
			.rollprice{
				display: flex;
				flex-direction: row;
				justify-content: space-between;
				align-items: center;
				.price{
					width: 50%;
					display: flex;
					flex-direction: column;
				}
				.rollmall{
					width: 50%;
					height: 40upx;
					text-align: center;
					line-height: 40upx;
					color: #FFFFFF;
					background: linear-gradient(to right,#FE877B,#DC505D);
					border-radius: 10upx;
				}
			}
		}
	}
</style>
