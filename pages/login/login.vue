<template>
	<view class="login">
		<image src="../../static/xiong1.png" mode="widthFix"></image>
		<button class='testbutton' type="primary" open-type="getUserInfo" @getuserinfo="mpGetUserInfo" withCredentials="true" v-if="userName==''">登录到小程序</button>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				userImg:'',
				userName:''
			}
		},
		onLoad() {
		   
		 },
		methods:{
			logins(){
				uni.switchTab({
					url:'../personal/personal'
				})
			},
			mpGetUserInfo(result) {
			let that=this;
			console.log('mpGetUserInfo', result);
			this.userName=result.mp.detail.userInfo.nickName;
			let userimgUrl=result.mp.detail.userInfo.avatarUrl;
			// 存储用户头像
			uni.setStorageSync('userimgUrl',userimgUrl)
			this.userImg=uni.getStorageSync('userimgUrl');
			// 存储用户昵称
			uni.setStorageSync('userName',this.userName);
			if (result.detail.errMsg !== 'getUserInfo:ok') {
				uni.showModal({
					title: '获取用户信息失败',
					content: '错误原因' + result.detail.errMsg,
					showCancel: false
				});
				return;
			}else{
				uni.login({
				  provider: 'weixin',
				  success: function (loginRes) {
					console.log(loginRes);
					let weixiCode=loginRes.code;
					// 存储微信code
					uni.setStorageSync('weixiCode',weixiCode);
					uni.request({
						url: 'http://appserver.wujie520.cn/thirdreturn/index/getopenid?code='+weixiCode,
						method: 'GET',
						success: res => {
							console.log(JSON.parse(res.data))
							let weixiOpenId=JSON.parse(res.data).openid;
							// 存储openid
							uni.setStorageSync('weixiOpenId',weixiOpenId);
							let name=uni.getStorageSync('userName');
							let parentid=uni.getStorageSync('partId');
							uni.request({
								url: 'http://appserver.wujie520.cn/thirdreturn/index/wxlogin?openid='+weixiOpenId+'&name='+name+'&parentid='+parentid,
								method: 'GET',
								success: res => {
									console.log(res.data)
									uni.setStorageSync("userId",res.data.data.user_id);
									that.userId=res.data.data.user_id;
									uni.setStorageSync("parentId",res.data.data.parent_id);
									uni.navigateBack();
								}
							});
						}
					});
				  }
				});
			}
			this.hasUserInfo = true;
			this.userInfo = result.detail.userInfo;
			console.log(this.userInfo)
		}
	}
	}
</script>

<style>
	.login{
		width: 320upx;
		height: 320upx;
		text-align: center;
		position: absolute;
		top: 0;
		bottom: 0;
		right: 0;
		left: 0;
		margin: auto;
	}
	.login>image{
		width: 160upx;
		height: 160upx;
		border-radius: 50%;
		background-color: black;
		margin-bottom: 30upx;
	}
</style>
