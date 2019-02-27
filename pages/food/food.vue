<template>
	<view class="food">
		<view class="foodlist">
			<view class="menumsg" v-for="(item,index) in propsdata==''?data:propsdata" :key="index"
			@click="goodfoodList(item,index)">{{item.opt_name}}</view>
			<!-- <view class="menumsg" ><text>查看更多</text></view> -->
		</view>
		<view class="shop">
			<image src="../../static/shop2.png" mode="widthFix"></image>
		</view>
		<view :class="fixmenu?'decscmenu descposit':'decscmenu'">
			<scroll-view scroll-x  class="scrollmenu">
				<view class="scrollview" v-for="(item,index) in descmenu" :key="index" @click="menuclicks(index)" 
				:class="Tabindex==index?'actives':''">{{item.name}}</view>
			</scroll-view>
		</view>
			<view class="goodLM" v-for="(item,index) in propsdata2" :key='index' @click="goodLm(item)">
			<image :src="item.goods_thumbnail_url" mode="widthFix"></image>
			<view class="goodLmdesc">
				<!-- 商品名字 -->
				<text class="title">{{item.goods_name}}</text>
				<!-- 领劵 拼的人数 -->
				<view class="juan">
					<text>卷{{item.coupon_min_order_amount/100}}</text>
					<text>已拼{{item.sold_quantity}}人</text>
				</view>
				<!-- 原价格  打折后的价格及跳转-->
				<view class="ling">
					<view class="yuan">
						<text>原价{{item.min_normal_price/100}}</text>
						<text>现价{{item.min_normal_price/100-item.coupon_min_order_amount/100}}</text>
					</view>
					<view class="tiao">
						{{item.coupon_min_order_amount/100}}
					</view>
				</view>
			</view>
		</view>
		美食
	</view>
</template>

<script>
	export default{
		data(){
			return{
				data:[1,2,3,4,5,6,7,8,9,10],
				Tabindex:0,
				descmenu:[
					{name:"综合"},
				    {name:"价格"},
					{name:"销量"}
				],
				product:[1,2,3,4,5],
				fixmenu:false
			}
		},
		props:{
			propsdata:{
				type:Object,
				defualt:function(e){
					return {}
				}
			},
			propsdata2:{
				type:Object,
				defualt:function(e){
					return {}
				}
			}
		},
		methods:{
			menuclicks(i){
				this.Tabindex=i;
			},
			goodfoodList(t,i){
				uni.setStorageSync('optId',t.opt_id);
					uni.navigateTo({
						url:"../gooList/gooList?id="+t.opt_id+'&opid='+t.topTid+'&name='+t.opt_name
					})
			}
		},
		// 在微信和app中可以用h5无效果
		onPageScroll(e) {
			// console.log(e);
			if(e.scrollTop>=240){
				 this.fixmenu=true;
			}else{
				this.fixmenu=false;
			}
		},
		onLoad() {
			
		}
	}
</script>

<style lang="less">
.descposit{
	position: fixed;
	top:180upx;
	width: 100%;
}
.goodLM{
	width: 100%;
	display: flex;
	flex-direction:row;
	margin-bottom:20upx;
	>image{
		width: 28%;
		height: 160upx;
	}
	.goodLmdesc{
		width: 72%;
		height: 160upx;
		display:flex;
		flex-direction:column;
		font-size:24upx;
		// 商品名字
		.title{
			height: 64upx;
		}
		// 领劵 拼的人数 
		.juan{
			width: 100%;
			height: 24upx;
			display:flex;
			flex-direction:row;
			justify-content:space-between;
		}
		// 原价格  打折后的价格及跳转
		.ling{
			width: 100%;
			height: 72upx;
			display:flex;
			flex-direction:row;
			justify-content:space-between;
			align-items: center;
			// 原价格  打折后的价格
			.yuan{
				display:flex;
				flex-direction:column;
			}
			.tiao{
				width: 40%;
				height: 50upx;
				text-align: center;
				line-height:50upx;
				background: linear-gradient(to right,#FFCB00,#FEA513);
				border-radius:12upx;
				color:#ffffff;
			}
		}
	}
}
</style>
