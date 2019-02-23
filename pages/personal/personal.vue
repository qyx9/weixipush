<template>
	<view class="personal">
		<view class="userinfo">
			<view class="userchanse">
				<view class="userList">
					<image :src="mess==true?userImg:userImg" mode="widthFix" v-show="userName!=''"></image>
					<!-- 登录状态 -->
					 <button class='testbutton' open-type="getUserInfo" @getuserinfo="mpGetUserInfo" withCredentials="true" v-if="userName==''">获取微信用户信息</button>
					<view class="usernames" v-if="userName!=''">
						<text>{{mess==true?userName:userInfo.nickName}}</text>
						<text>ID:{{userId}}</text>
					</view>
				</view>
				<view class="userList">
					<view class="loginBind">
						<image src="../../static/phone.png" mode="widthFix"></image>
						<text>绑手机号</text>
					</view>
					<view class="loginBind">
						<image src="../../static/weixin.png" mode="widthFix"></image>
						<text>绑微信号</text>
					</view>
				</view>
			</view>
			<view class="userchanse">
				<view class="accountBalance">
					<text>账户余额</text>
					<text>{{mymoney/100?mymoney/100:0}}</text>
				</view>
				<view class="rests">
					<view class="accountBalance">
						<text>账户余额</text>
						<text>0</text>
					</view>
					<view class="accountBalance">
						<text>账户余额</text>
						<text>0</text>
					</view>
				</view>
			</view>
		</view>
		<view class="perMenu">
			<view class="setMenu">
				<text>可提现金额</text>
				<text>0</text>
			</view>
			<view class="mingxi">
				<view><text>明细</text></view>
				<view><text>提现</text></view>
			</view>
		</view>
		<!-- 用户管理 -->
		<view class="usermangent">
			<view class="title">用户管理</view>
			<view class="userlist">
				<text>推荐人:</text>
			</view>
		</view>
		<!-- 粉丝数量 -->
		<view class="usermangent">
			<view class="title">我的粉丝</view>
			<view class="userlist">
				<text>数量:{{yourf}}</text>
			</view>
		</view>
		<!-- 清除数据 -->
		<view class="usermangent" @click="cleardata">
			<view class="title" >清除数据</view>
			<view class="userlist">
				<text>清除数据</text>
			</view>
		</view>
		<!-- 订单详情 -->
		<view class="usermangent" @click="ordermess">
			<view class="title" >我的订单</view>
			<view class="userlist">
				<text>订单数据</text>
			</view>
		</view>
		<view class="like">
			<image src="../../static/like.png" mode="widthFix"></image>
		</view>
		<view class="maybelike">
			<view class="rempro" v-for="(item,index) in data" :key="index"></view>
		</view>
		personal
	</view>
</template>

<script>
	// 引入icon
	import UniIcon from '@/components/uni-icon.vue'
	export default {
		components:{
			UniIcon
		},
		data() {
			return {
				imgs:'',
				data:[1,2,3,4,5,6],
				userName:'',
				userId:'',
				mymoney:0,
				yourf:0,
				userImg:'',
				defualtImg:'../../static/girl.png',
				iconimg:false,
				userInfo:"",
				mess:false
			}
		},
		onLoad() {
			let that=this;
		uni.getSetting({
		   success(res) {
			  console.log(res.authSetting)
		   }
		   
		})
		},
		methods: {
			mymoneys(){
				let open=uni.getStorageSync('weixiOpenId')
				uni.request({
					url: 'http://appserver.wujie520.cn/thirdreturn/index/wxintegral?openid='+open,
					method: 'GET',
					data: {},
					success: res => {
						this.mymoney=res.data.data.integral;	
						console.log(this.mymoney)				
					}
				});
			},
			feshi(){
				let open=uni.getStorageSync('weixiOpenId')
				uni.request({
					url: 'http://appserver.wujie520.cn/thirdreturn/index/wxfans?openid='+open,
					method: 'GET',
					data: {},
					success: res => {
						this.yourf=res.data.data.fans_count;
					}
				});
			},
			mpGetUserInfo(result) {
				let that=this;
				console.log('mpGetUserInfo', result);
				this.userName=result.mp.detail.userInfo.nickName;
				let userimgUrl=result.mp.detail.userInfo.avatarUrl;
				uni.setStorageSync('userimgUrl',userimgUrl)
				this.userImg=uni.getStorageSync('userimgUrl');
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
						uni.setStorageSync('weixiCode',weixiCode);
						uni.request({
							url: 'http://appserver.wujie520.cn/thirdreturn/index/getopenid?code='+weixiCode,
							method: 'GET',
							success: res => {
								console.log(JSON.parse(res.data))
								let weixiOpenId=JSON.parse(res.data).openid;
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
// 										that.mymoneys();
// 										that.feshi();
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
			},
			// 清除数据
			cleardata(){
				uni.showModal({
				title: '温馨提示',
				content: '你确定要清除数据吗',
				showCancel: true,
				success: function (res) {
					if (res.confirm) {
						uni.clearStorage()
					} else if (res.cancel) {
						console.log('用户点击取消');
					}
                   }
			});
			},
			// 订单
			ordermess(){
				uni.navigateTo({
					url: '../ordermess/ordermess?id=15'
				});
			}
			
		},
		destroyed() {
			
		},
		onShow() {
			this.mess=true;
			this.mymoneys();
			this.feshi();
			this.userId=uni.getStorageSync('userId');
			this.userName=uni.getStorageSync('userName');
			this.userImg=uni.getStorageSync('userimgUrl');
		},
		onTabItemTap(){
			this.mymoneys();
			this.feshi();
		}
	}
</script>

<style>
	.personal {
		height: 4000upx;
		background-color:#EEEEEE;
	}
	.userinfo{
		width: 100%;
		height: 300upx;
		background:url(../../static/bg.png);
	}
	.perMenu{
		width: 100%;
		height: 154upx;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 20upx;
	}
	/* 提现 */
	.perMenu .setMenu{
		width: 50%;
		display: flex;
		flex-direction: column;
		text-align: left;
	}
	.perMenu .mingxi{
		width: 50%;
		display: flex;
		flex-direction: row;
	}
	.mingxi>view{
		width: 40%;
		text-align: center;
		font-size: 30upx;
		height: 40upx;
		margin: 0 10upx;
		border:2upx solid #DC505D;
	}
	.mingxi>view:nth-child(2){
		background-color: #DC505D;
		color:#FFFFFF;
		border-radius: 10upx;
	}
	.like>image{
		width: 100%;
		height: 46upx;
	}
	.maybelike{
		box-sizing: border-box;
		width: 100%;
		padding: 10upx 2%;
		display: flex;
		flex-direction:row;
		flex-wrap:wrap;
		justify-content: space-between;
	}
	.rempro{
		width: 48%;
		height: 268upx;
		background-color: blue;
		border-radius: 10upx;
		margin-bottom: 20upx;
	}
	/* userone */
	.userchanse{
		width: 100%;
		height: 150upx;
		display: flex;
		flex-direction: row;
		color: #CCCCCC;
	}
	.userList{
		box-sizing:border-box;
		width: 50%;
		height: 150upx;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}
	.userList>image{
		width: 120upx;
		height: 120upx;
		border-radius: 50%;
		margin-right: 20upx;
	}
	.usernames{
		font-size:32upx;
		width: 50%;
		display: flex;
		flex-direction:column;
		justify-content: center;
	}
	.loginBind{
		font-size:24upx;
		width: 50%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.loginBind>image{
		width: 60upx;
		height: 60upx;
	}
	/* user two */
	.accountBalance{
		width: 50%;
		height: 150upx;
		display: flex;
		flex-direction: column;
	}
	.accountBalance>text:first-child{
		font-size: 32upx;
	}
	.accountBalance>text:nth-child(2){
		color: red;
		font-size: 64upx;
	}
	.rests{
		width: 50%;
		height: 150upx;
		display:flex;
		flex-direction:row;
	}
	.rests>.accountBalance{
		font-size: 24upx;
	}
	.rests>.accountBalance:nth-child(2){
		border-left: 2upx solid #555555;
		height: 100upx;
	}
	/* 可提现金额 */
	.setMenuMsg{
		width: 48%;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}
	/* 用户管理 */
	.usermangent{
		width: 100%;
		height:120upx;
		background-color: #FFFFFF;
		margin-bottom: 20upx;
	}
</style>
