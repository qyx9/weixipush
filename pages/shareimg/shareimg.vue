<template>
	<view class="shareimg">
		<image :src="imgurl" mode="widthFix"></image>
		<picture-tal :pictureSrc="imgurl"></picture-tal>
	</view>
</template>

<script>
	import pictureTal from '../../components/picture-tailor/pictureTailor.vue'
	export default {
		components:{
			pictureTal
		},
		data(){
			return{
				imgurl:'../../static/timg.jpg'
			}
		},
		methods:{
			
		},
		onLoad(){
			var that=this;
// 			uni.showLoading({
// 				title:'图片保存中...'
// 			})
			uni.downloadFile({
				url:'http://192.168.1.52:8080/#/pages/shareimg/shareimg',
				success(res) {
					console.log(res.tempFilePath)
						uni.saveImageToPhotosAlbum({
						filePath:res.tempFilePath,
						success(res) {
							uni.showToast({
								title:'保存成功',
								success() {
									// uni.hideLoading();
									uni.navigateBack();
								}
							})
							console.log(res.savedFilePath);
						},
						fail(err) {
							console.log(err)
							uni.showToast({
								icon:'none',
								title:'保存失败',
								success() {
									uni.hideLoading();
									// uni.navigateBack();
								}
							})
						}
					})
				}
			})
		}
	}
</script>

<style lang="less">
	.shareimg{
		// 图片
		>image{
			width: 100%;
		}
	}
</style>
