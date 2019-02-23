<template>
	<view class="food">
		<view class="foodlist">
			<view class="menumsg" v-for="(item,index) in data" :key="index">{{item}}</view>
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
		<view class="porduct">
			<view class="productList" v-for="(item,index) in product" :key="index">
				<image src="../../static/product.png" mode="widthFix"></image>
				<view class="msg">台湾脆梨</view>
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
		methods:{
			menuclicks(i){
				this.Tabindex=i;
			}
		},
		onPageScroll() {
			uni.createSelectorQuery().selectViewport().scrollOffset(res => {
				  if(res.scrollTop>260){
					 this.fixmenu=true;
				  }
				  else{
					  this.fixmenu=false;
				  }
				}).exec();
		},
		onLoad() {
			uni.hideLoading();
		}
	}
</script>

<style>
.descposit{
	position: fixed;
	top:180upx;
	width: 100%;
}
</style>
