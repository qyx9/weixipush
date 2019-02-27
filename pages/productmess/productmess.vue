<template>
	<view class="productmess" v-show="loding">
		<view class="goodlists">
			<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" circular>
				<swiper-item v-for="(item,index) in bannerdata" :key="index">
					<image :src="item" mode="widthFix" class="imgsty" @click="preimg(item)"></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="jiage">
			<view class="jflex">
				<text>劵￥{{data.coupon_min_order_amount/100}}</text>
				<text>原价￥{{data.min_normal_price/100}}</text>
			</view>
			<view class="jflex">
				<text>有效期</text>
				<text>卷后价格{{(data.min_normal_price-data.coupon_discount)/100}}</text>
			</view>
		</view>
		<view class="goodname">
			<text>{{data.goods_name}}</text>
		</view>
		<view class="mallnum">
			<text>已售卖{{data.sold_quantity}}件</text>
		</view>
		<view class="roll" @click="roll">
			<view class="rollnum">
				<text>￥{{data.coupon_discount/100}}</text>
				<view class="rolldata">
					<view><text>优惠卷</text></view>
					<view><text>有效期01.01-01.01</text></view>
				</view>
			</view>
			<text>立即领卷</text>
		</view>
		<view class="rollCount">
		   <text>总优惠卷{{data.coupon_total_quantity==null?0:data.coupon_total_quantity}}</text>	
		   <text>剩余张{{data.coupon_remain_quantity==null?0:data.coupon_remain_quantity}}</text>	
		</view>
		<view class="goodscore">
			<view class="scoretitle">
				<text>商品综合评分{{zongscore}}分,满分5分</text>
			</view>
			<view class="score">
				<view class="scoremsg">
					<text>服务评分{{data.avg_serv/100}}</text>
					<text>描述评分{{data.avg_desc/100}}</text>
					<text>物流评分{{data.avg_lgst/100}}</text>
				</view>
				<view class="scoremsg2">
					<view class="beat">
						<text>击败</text>
						<view class="beatbg">
							<view :style="{backgroundColor:'#FEB04C', width:serpct+'%',height: '100%'}"></view>
						</view>
						<text>{{serpct}}%</text>
					</view>
					<view class="beat">
						<text>击败</text>
						<view class="beatbg">
							<view :style="{backgroundColor:'#FEB04C', width:descpct+'%',height: '100%'}"></view>
						</view>
						<text>{{descpct}}%</text>
					</view>
					<view class="beat">
						<text>击败</text>
						<view class="beatbg">
							<view :style="{backgroundColor:'#FEB04C', width:lgstpct+'%',height: '100%'}"></view>
						</view>
						<text>{{lgstpct}}%</text>
					</view>
				</view>
				<view class="scoremsg3">
					<text>同类店铺</text>
					<text>同类店铺</text>
					<text>同类店铺</text>
				</view>
			</view>
		</view>
		<!-- 商品评价 -->
		<view class="reviews">
			<text>商品评价</text>
			<view class="allmsg" @click="lookall"><text>{{look==false?'查看全部>':'收起评论V'}}</text></view>
		</view>
		<view class="userReviews" v-show="look">用户评论</view>
		<!-- 商品信息 -->
		<view class="gooddesc">
			<!-- 商品名 -->
			<view><text>{{data.goods_desc}}</text></view>
			<view class="descimg" v-for="(item,index) in bannerdata" :key="index">
				<image :src="item" mode="widthFix" @click="preimg(item)"></image>
			</view>
		</view>
		<!-- 弹出层 -->
		<view class="uni-banner" style="background:#FFFFFF;" v-if="bannerShow">
		    <view style="justify-content:flex-end;" @tap="closeBanner">
		        <view style="justify-content:flex-end; text-align:right; padding:20upx;">
		            <text class="uni-icon uni-icon-close"></text>
		        </view>
		    </view>
		    <view>
		        <image src="https://img-cdn-qiniu.dcloud.net.cn/uniapp/images/shuijiao.jpg" style="width:100%;" mode="widthFix"></image>
		    </view>
		    <view style="padding:50upx 0; padding-bottom:68upx;">
		        <button type='warn' class="mini-btn" style="background:#F6644D; margin:0 80upx;">一个按钮</button>
		    </view>
		</view>
		<view class="uni-mask" v-if="bannerShow"></view>
		<!-- 弹出层 -->
		<!-- bottombar -->
		<view class="bottombar">
			<view class="indexbar" @click="indexurl">
				<text>首页</text>
			</view>
			<view class="sharemess" @click="sharefriend">
			     <button open-type="share">分享给好友购买</button>	
			</view>
			<view class="sharemess" @click="roll">
				<text>立即领劵购买</text>	
			</view>
		</view>
		产品详情
	</view>
</template>

<script>
	export default{
		data(){
			return {
				data:[],
				bannerdata:[],
				dataurl:'',
				appid:'',
				pathges:'',
				loding:false,
				zongscore:'',
				look:false,
				serpct:'',
				descpct:'',
				lgstpct:'',
				bannerShow:false
			}
		},
		onLoad:function(e){
			uni.showLoading({
				title:"加载中..."
			})
			console.log(e);
			let goodId=e.id;
			// 存储goodId
			uni.setStorageSync('goodId',goodId);
			let openid=uni.getStorageSync('weixiOpenId');
			if(openid==''){
				uni.hideLoading();
				uni.showModal({
					title: '提示',
					content: '微信未登录小程序,请先登录后继续操作',
					showCancel: false,
					success: res => {
						if(res.confirm){
							uni.navigateTo({
								url:'../login/login'
							})
						}
					}
				});
			}else{
			uni.request({
				url: 'http://appserver.wujie520.cn/thirdreturn/index/getgoodsdetail?goods_id='+e.id+'&openid='+openid,
				method: 'GET',
				data: {},
				success: res => {
					uni.hideLoading();
					console.log(res.data)
					this.data=res.data.goods_detail.goods_detail_response.goods_details[0];
					this.bannerdata=res.data.goods_detail.goods_detail_response.goods_details[0].goods_gallery_urls;
					this.dataurl=res.data.goods_url.goods_promotion_url_generate_response.goods_promotion_url_list[0].short_url;
					this.appid=res.data.goods_url.goods_promotion_url_generate_response.goods_promotion_url_list[0].we_app_info.app_id;
					this.pathges=res.data.goods_url.goods_promotion_url_generate_response.goods_promotion_url_list[0].we_app_info.page_path;
					this.loding=true;
					this.zongscore=(this.data.avg_desc+this.data.avg_serv+this.data.avg_lgst)/300;
					this.zongscore=Math.floor(this.zongscore*100)/100;
					this.serpct=Math.floor(this.data.serv_pct*100*100)/100;
					this.descpct=Math.floor(this.data.desc_pct*100*100)/100;
					this.lgstpct=Math.floor(this.data.lgst_pct*100*100)/100;
				}
			});
			}
		},
		methods:{
			preimg(i){
				let urlimg=i;
				console.log(urlimg);
				uni.previewImage({
					urls:this.bannerdata,
					current:urlimg
				})
			},
			roll(){
				// 小程序跳转
				uni.navigateToMiniProgram({
					  appId: this.appid,
					  path: this.pathges,
					  success(res) {
						// 打开成功
						console.log('success');
					  }
					})
			},
			lookall(){
				this.look=!this.look
			},
			sharefriend(){
				
			},
			indexurl(){
				uni.switchTab({
					url: '../index/index'
				});
				console.log('1');
			}
		},
		destroyed() {
			
		},
		onShow() {
			let openid=uni.getStorageSync('weixiOpenId');
			let e=uni.getStorageSync('goodId');
			uni.request({
				url: 'http://appserver.wujie520.cn/thirdreturn/index/getgoodsdetail?goods_id='+e+'&openid='+openid,
				method: 'GET',
				data: {},
				success: res => {
					uni.hideLoading();
					console.log(res.data)
					this.data=res.data.goods_detail.goods_detail_response.goods_details[0];
					this.bannerdata=res.data.goods_detail.goods_detail_response.goods_details[0].goods_gallery_urls;
					this.dataurl=res.data.goods_url.goods_promotion_url_generate_response.goods_promotion_url_list[0].short_url;
					this.appid=res.data.goods_url.goods_promotion_url_generate_response.goods_promotion_url_list[0].we_app_info.app_id;
					this.pathges=res.data.goods_url.goods_promotion_url_generate_response.goods_promotion_url_list[0].we_app_info.page_path;
					this.loding=true;
					this.zongscore=(this.data.avg_desc+this.data.avg_serv+this.data.avg_lgst)/300;
					this.zongscore=Math.floor(this.zongscore*100)/100;
					this.serpct=Math.floor(this.data.serv_pct*100*100)/100;
					this.descpct=Math.floor(this.data.desc_pct*100*100)/100;
					this.lgstpct=Math.floor(this.data.lgst_pct*100*100)/100;
				}
			});
			}
	}
</script>

<style>
	/* .productmess>.uni-swiper-wrapper{
		height: 200%;
	} */
	/* .productmess>swiper{
		height: 600upx;
	} */
		
	.productmess{
		height: 4000upx;
		background-color: #EEEEEE;
	}
	.jiage,.rollCount,.goodscore,.roll,.goodname,.mallnum,.gooddesc,.bottombar{
		background-color: #FFFFFF;
	}
	.imgsty{
		width: 100%;
		height: 320upx;
	}
	.jiage{
		width: 100%;
	}
	.jflex{
		height: 30upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	.roll{
		box-sizing: border-box;
		padding: 0 20upx;
		width: 100%;
		color:#FAE6E3;
		height: 160upx;
		background: linear-gradient(to right,#DC505D,#FE877B);
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	.rollnum{
		width: 60%;
		display: flex;
		flex-direction: row;
	}
	.rollCount{
		width: 100%;
		text-align: center;
		margin-bottom: 30upx;
	}
	/* 物流 */
	.goodscore{
		font-size: 32upx;
		margin-bottom: 20upx;
	}
	.scoretitle{
		text-align: center;
	}
	.score{
		width: 100%;
		height: 200upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	.scoremsg{
		width: 26%;
		height: 180upx;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}
	.scoremsg2{
		width: 56%;
		font-size: 24upx;
		 height: 180upx;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}
	.scoremsg3{
		width: 18%;
        height: 180upx;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}
	.beat{
		display: flex;
		flex-direction: row;
	}
	.beat>text{
		width: 20%;
	}
	.beatbg{
		width: 60%;
		height: 30upx;
		background-color: #EFF2F7;
		border-radius: 8upx;
	}
	/* 商品评价 */
	.reviews{
		width: 100%;
		height: 60upx;
		border-left:4upx solid #FEB04C;
		background-color: #FFFFFF;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
	}
	.reviews .allmsg{
		width: 50%;
		height: 60upx;
		text-align: right;
	}
	/* 商品信息 */
	.gooddesc .descimg>image{
		width: 100%;
		height: 600upx;
		margin-top: 20upx;
	}
	/* bottombar */
	.productmess .bottombar{
		position: fixed;
		bottom: 0;
		width: 100%;
		height: 120upx;
		color:#CCCCCC;
		display: flex;
		flex-direction: row;
	}
	.productmess .bottombar .indexbar{
		width: 20%;
	}
	.productmess .bottombar .sharemess:nth-child(2){
		width: 40%;
		background-color: #FEB04C;
		line-height: 120upx;
		color: #FFFFFF;
		text-align: center;
	}
	.productmess .bottombar .sharemess:nth-child(2)>button{
		border: none;
		background-color: #FEB04C;
		line-height: 120upx;
		color: #FFFFFF;
	}
	.productmess .bottombar .sharemess:nth-child(3){
		width: 40%;
		background-color: #FD4F42;
		line-height: 120upx;
		color: #FFFFFF;
		text-align: center;
	}
	 /* 遮罩层 */
	.uni-mask {
	    background: rgba(0, 0, 0, 0.6);
	    position: fixed;
	    width: 100%;
	    height: 100%;
	    left: 0;
	    top: 0;
	    z-index: 1;
	}
	
	/* 弹出层形式的广告 */
	.uni-banner {
	    width: 70%;
	    position: fixed;
	    left: 50%;
	    top: 50%;
	    background: #FFF;
	    border-radius: 10upx;
	    z-index: 99;
	    transform: translate(-50%, -50%);
	}
</style>
