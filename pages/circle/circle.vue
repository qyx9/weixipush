<template>
	<view class="circle">
		<view class="cirbg"></view>
		<view class="featuredProducts">
			<view class="featitle">
				<view class="left">
					<image src="../../static/icons.png" mode="widthFix"></image>
					<text>小编推荐</text>
				</view>
				<button class="right" @click="share">一键分享</button>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {

		},
		methods: {
           share(){
			   
		   }
		}
	}
</script>
<style>
	.circle {
		padding-top:64upx;
		height: 4000upx;
	}
	.cirbg{
		width: 100%;
		background-color: #FFFFFF;
		height: 64upx;
		position: fixed;
		top: 0;
	}
	.featitle{
		box-sizing: border-box;
		font-size: 30upx;
		padding: 0 40upx;
		width:100%;
		height: 58upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}
	.featitle .left{
		width: 30%;
		height: 58upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	.featitle .left>text{
		width: 40;
		padding-top: 14upx;
	}
	.featitle .left>image{
		width: 58upx;
		height: 58upx;
	}
	/* .featitle .right{
		width: 30%;
		text-align:center;
		height: 40upx;
		border:2upx #CCCCCC solid;
		border-radius: 24upx;
	} */
</style>
