<template>
	<view class="indexs">
		<!-- search -->
		<view class="index">
			<view class="imgicon">
				<image src="../../static/duo.png" mode="widthFix"></image>
			</view>
			<view class="inputs">
				<input class="inputbox" type="text" value="" placeholder="输入标题或商品关键字" @click="searchbox"/>
				</view>
			<view class="imgicon">
				<image src="../../static/lingdang.png" mode="widthFix"></image>
			</view>
		</view>
		<!-- submenu -->
		<scroll-view id="tab-bar" class="uni-swiper-tab" scroll-x >
			<view v-for="(tab,index) in tabmenus" :key="index" :class="['swiper-tab-list',tabIndex==index ? 'active' : '']"
			 :id="index" :data-current="index" @click="tapTab(tab,index)">{{tab.opt_name}}</view>
		</scroll-view>
		<!-- submenu list -->
		<scroll-view class="list" v-for="(item,index) in msg" v-if="tabIndex==index" :key="index" scroll-y >
		     <recom-day v-if="index===0"></recom-day>
			 <food-list :propsdata='propsdata' :propsdata2="propsdata2" v-if="index===1"></food-list>
			 <infant-bady :propsdata='propsdata'  v-if="index===2"></infant-bady>
			 <fruits-list v-if="index===3"></fruits-list>
			 <costume-list v-if="index===4"></costume-list>
	    </scroll-view>
		<view class="jiantou" @click="topzero" v-show="topbolen">
			<image src="../../static/jiantou.png" mode="widthFix"></image>
		</view>
		
	</view>
</template>

<script>
	import RecomDay from '../recommend/recommend.vue'
	import FoodList from '../food/food.vue'
	import InfantBady from '../infant/infant.vue'
	import FruitsList from '../fruits/fruits.vue'
	import CostumeList from '../costume/costume.vue'
	var openid=uni.getStorageSync('weixiOpenId');
	export default {
		components:{
			RecomDay,
			FoodList,
			InfantBady,
			FruitsList,
			CostumeList
		},
		data() {
			return {
				data:[
					{
					  name:"今日推荐"
					},
					{
					  name:"美食"
					},
					{
					  name:"母婴"
					},
					{
					  name:"水果"
					},
					{
					  name:"服饰"
					}
				],
				msg:[
					{
					  name:"今日推荐"
					},
					{
					  name:"美食"
					},
					{
					  name:"母婴"
					},
					{
					  name:"水果"
					},
					{
					  name:"服饰"
					}
				],
				tabIndex:0,
				topbolen:false,
				parid:0,
				userId:5,
				shareId:'',
				userName:'0',
				tabmenus:[],
				propsdata:'',
				propsdata2:''
		
			}
		},
		onLoad:function (e) {
			console.log(e);
			
			let result=e;
			uni.setStorageSync('partId',e.id);
			console.log("分享的id",uni.getStorageSync('partId'));
			let openid=uni.getStorageSync('weixiOpenId');
			if(openid==''){
			uni.showModal({
				title: '温馨提示',
				content: '分享时请先登录小程序',
				showCancel: false,
				success() {
					if(result){
						uni.setStorageSync('partId',result.id);
						let n=uni.getStorageSync('partId');
						console.log('partId',n);
						return;
					}
				}
			});
			}
			let  that=this
			this.userName=uni.getStorageSync('userName')
			this.tabmenuList();
		},
		onPageScroll(e) {
			 if(e.scrollTop>480){
				this.topbolen=true;
			}
			else{
			   this.topbolen=false;
			}
// 			uni.createSelectorQuery().selectViewport().scrollOffset(res => {
// 			  if(res.scrollTop>480){
// 				  this.topbolen=true;
// 			  }
// 			  else{
// 				  this.topbolen=false;
// 			  }
// 			}).exec();
		},
		methods: {
           searchbox(){
			   uni.navigateTo({
			   	url: '../HM-search/HM-search'
			   });
		   },
		   tapTab(t,i){
			   this.tabIndex=i;
			   var optId=t.opt_id;
			   console.log(this.tabIndex)
			   uni.request({
			   	url: 'http://appserver.wujie520.cn/thirdreturn/index/classgoods?class_id='+t.opt_id+'&page=1&is_sub=1&prent_id='+t.opt_id,
			   	method: 'GET',
			   	data: {},
			   	success: res => {
					console.log(res.data)
					this.propsdata=res.data[0].goods_opt_get_response.goods_opt_list.splice(0,9);
					this.propsdata.push({opt_name:'查看更多',topTid:optId});
					this.propsdata2=res.data[1].goods_search_response.goods_list;
				},
			   	fail: () => {},
			   	complete: () => {}
			   });
			  uni.pageScrollTo({
					scrollTop: 0,
					duration: 10
				});
		   },
		   topzero(){
			   uni.pageScrollTo({
			   	scrollTop:0,
				duration:300
			   })
		   },
		   changeTab(e){
			   console.log(e)
		   },
		   // 菜单目录
		   tabmenuList(){
			   uni.request({
			   	url: 'http://appserver.wujie520.cn/thirdreturn/index/goodsopt',
			   	method: 'GET',
			   	data: {},
			   	success: res => {
					console.log(res.data);
					this.tabmenus=res.data.goods_opt_get_response.goods_opt_list;
				},
			   	fail: () => {},
			   	complete: () => {}
			   });
		   }
		   
		},
		// 分享的id
		onShareAppMessage(){
			var id=uni.getStorageSync('userId');
			return {
				  title: '自定义转发标题',
				  path: 'pages/index/index?ider=0&id='+id
				}
		},
		onTabItemTap(){
			this.userId=uni.getStorageSync('userId');
			uni.setStorageSync('sharId',this.userId);
			this.shareId=this.userId;
			console.log(this.shareId)
		}
	}
</script>

<style>
	.indexs{
		height: 8000upx;
	}
	.index{
		width: 100%;
		height: 80upx;
		display: flex;
		flex-direction:row;
		justify-content: space-between;
		align-items: center;
		background-color: #FFD305;
		position: fixed;
		top: 0;
		z-index: 100;
	}
.inputs{
	width: 70%;
}
.inputbox{
	height: 16upx;
	font-size: 24upx;
	border-radius: 14upx;
	text-indent: 6upx;
	background-color: #FFFFFF;
}
.imgicon{
	width: 15%;
	text-align: center;
}
.imgicon>image{
	width: 68upx;
	height: 80upx;
}
.uni-tab-bar {
	display: flex;
	flex: 1;
	flex-direction: column;
	overflow: hidden;
	height: 100%;
}

.list {
	padding-top: 180upx;
	width: 750upx;
	height: 100%;
}
.clearfix:after{
	display: block;
	content: "";
	height: 0;
	clear: both;
	visibility: hidden;
}
.uni-swiper-tab {
	width: 100%;
	white-space: nowrap;
	line-height: 100upx;
	height: 100upx;
	position: fixed;
	top: 80upx;
	z-index: 100;
	background-color: #FFFFFF;
}
.swiper-tab-list:first-child{
	height: 30upx;
	line-height: 30upx;
	border-right: #8F8F94 solid 1upx;
}
.swiper-tab-list {
	font-size: 30upx;
	width: 150upx;
	display: inline-block;
	text-align: center;
	color: #555;
}

.active {
	color: #FFD305;
}

.uni-tab-bar .swiper-box {
	flex: 1;
	width: 100%;
	height: calc(100% - 100upx);
}

.uni-tab-bar-loading{
	padding:20upx 0;
}
.jiantou{
	width: 50upx;
	height: 50upx;
	position: fixed;
	bottom: 10upx;
	right: 10upx;
}
.jiantou>image{
	width: 50upx;
	height: 50upx;
	border-radius: 50%;
	border: 2upx solid #CCCCCC;
}
</style>
