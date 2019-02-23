<template>
	<view class="ordermess">
		<view class="usermess">
			<image :src="userImg" mode=""></image>
		    <view class="myname"><text>名字:</text><text>{{username}}</text></view>
		</view>
		<view class="myorder">
			<view class="title">
				<text>我的订单</text>
				<view class="lookorder" @click="lookorder"><text>{{order==false?'查看订单>':'收起订单V'}}</text></view>
			</view>
			<view v-if='order' class="myproduct" v-for="(item,index) in myproduct" :key='index' @click="productmess(item)">
				<image :src="item.goods_thumbnail_url" mode=""></image>
				<view class="goodname">
					<text>{{item.goods_name}}</text>
					<text>描述:{{item.order_status_desc}}</text>
				</view>
				<view class="goodmun">数量:{{item.goods_quantity}}</view>
			</view>
		</view>
		<view class="fanorder">
			<view class="title">
				<text>粉丝订单</text>
				<view class="lookorder" @click="lookfan"><text>{{fanorder==false?'查看订单>':'收起订单V'}}</text></view>
			</view>
			<view v-if='fanorder' class="myproduct" v-for="(item,index) in fanproduct" :key='index' @click="productmess(item)">
				<image :src="item.goods_thumbnail_url" mode=""></image>
				<view class="goodname">
					<text>{{item.goods_name}}</text>
					<text>描述:{{item.order_status_desc}}</text>
				</view>
				<view class="goodmun">数量:{{item.goods_quantity}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
			      openid:'',
				  username:'',
				  userImg:'',
				  myproduct:'',
				  fanproduct:'',
				  order:false,
				  fanorder:false
			}
		},
		methods:{
			myoder(){
				uni.request({
					url: 'http://appserver.wujie520.cn/thirdreturn/index/wxorder?openid='+this.openid,
					method: 'GET',
					data: {},
					success: res => {
						console.log(res.data)
						this.myproduct=res.data.data.my_order;
						this.fanproduct=res.data.data.fans_order[2];
					}
				});
			},
			productmess(i){
				uni.navigateTo({
					url: '../productmess/productmess?id='+i.goods_id
				});
			},
			lookorder(){
				this.order=!this.order;
			},
			lookfan(){
				this.fanorder=!this.fanorder;
			}
		},
		onLoad(){
			this.openid=uni.getStorageSync('weixiOpenId');
			this.username=uni.getStorageSync('userName');
			this.userImg=uni.getStorageSync('userimgUrl');
			this.myoder();
		}
	}
</script>

<style>
	.ordermess{
		background-color: #CCCCCC;
	}
	.ordermess .usermess{
	   width: 100%;
	   height:180upx;
	   display: flex;
	   flex-direction:row;
	   justify-content:center;
	   align-items:center;
	   background-color:#FFFFFF;
	   margin-bottom: 20upx;
	}
	.ordermess .usermess image{
	   width: 140upx;
	   height:140upx;
	   border-radius: 50%;
	   margin-right: 30upx;
	}
	.ordermess .usermess .myname{
		width: 50%;
		background-color:#FFFFFF;
	}
	/* 我的商品 */
	.ordermess .myorder{
		box-sizing: border-box;
		width:100%;
        display: flex;
		flex-direction:column;
		align-items: center;
		border-bottom: 30upx;
		}
		.ordermess  .title{
			box-sizing: border-box;
			padding: 0 14upx;
			width:100%;
			height: 80upx;
			line-height: 80upx;
			font-weight: bold;
			background-color:#FFFFFF;
			margin-bottom: 20upx;
			border-radius: 14upx;
			display: flex;
			flex-direction:row;
			justify-content:space-between;
			align-items:center;
		}
	.ordermess  .myproduct{
		box-sizing: border-box;
		width: 100%;
		height:180upx;
		display: flex;
		flex-direction:row;
		justify-content:space-between;
		align-items:center;
		font-size: 24upx;
		background-color:#FFFFFF;
		margin-bottom: 20upx;
		padding: 0 10upx;
	}	
	.ordermess  .myproduct image{
	   width: 140upx;
	   height:140upx;
	   border-radius: 14upx;
	   margin-right: 30upx;
	}
	.ordermess  .myproduct .goodname{
		width: 70%;
		display: flex;
		flex-direction:column;
		justify-content:space-between;
		/* align-items:center; */
	}
	.ordermess  .myproduct .goodmun{
		width: 10%;
	}
	/* 粉丝订单 */
	.fanorder .fanorder{
		box-sizing: border-box;
		width:100%;
		display: flex;
		flex-direction:column;
		align-items: center;
		padding: 0 20upx;
	}
</style>
