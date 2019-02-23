<template>
	<view>
	<web-view :src="dataurl"></web-view>
	</view>
</template>

<script>
	export default{
		data(){
			return {
				dataurl:'https://a.toutiaonanren.com/api/d/ULDRtW'
			}
		},
		onLoad:function(e){
			console.log(e)
		}
	}
</script>

<style>
</style>
