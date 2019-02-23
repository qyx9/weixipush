<template>
	<view class="food">
		<view class="foodlist">
			<view class="menumsg" v-for="(item,index) in data" :key="index">{{item}}</view>
		</view>
		<view class="shop">
			<image src="../../static/shop2.png" mode="widthFix"></image>
		</view>
		<view class="decscmenu">
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
		水果
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
				product:[1,2,3,4,5]
			}
		},
		methods:{
			menuclicks(i){
				this.Tabindex=i;
			}
		}
	}
</script>

<style>

</style>
