<template>
	<view class="goodListMess">
		<scroll-view scroll-x class="uni-swiper-tab" v-if="topIDF">
			<view :class="topIndex==index?'active goodListMenu':'goodListMenu'" v-for="(item,index) in Catalog" :key="index" @click="catalog(item,index)"><text>{{item.opt_name}}</text></view>
		</scroll-view>
		<!-- 排序 -->
		<view :class="topIDF?'topPosition decscmenu':'NonePosition decscmenu'">
		    <view class="descName" v-for="(item,index) in descmenu" :key="index" @click="descMenu(item,index) " 
			 :class="tabIndex==index?'active':''"><text>{{item.name}}</text></view>                  
		</view>
		<!-- 距离排序的加价格距离 -->
		<view :class="topIDF?'allTopMargin':'topMargin'"></view>
		<view class="goodLM" v-for="(item,index) in data" :key='index' @click="goodLm(item)">
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
						<text>拼多多价{{item.min_normal_price/100}}</text>
						<text>卷后价{{item.min_normal_price/100-item.coupon_min_order_amount/100}}</text>
					</view>
					<view class="tiao">
						立即领卷{{item.coupon_min_order_amount/100}}元
					</view>
				</view>
			</view>
		</view>
		<load-mores v-if="showloadMore" :status="loadMoreText==='加载中...'?'loading':''" ></load-mores>
	</view>
</template>

<script>
	import LoadMores from '../../components/uni-load-more/uni-load-more.vue'
	export default{
		components:{
			LoadMores
		},
		data(){
			return{
				descmenu:[
					{name:"综合",num:0},
				    {name:"价格",num:3},
					{name:"销量",num:6}
				],
				propsdata:'',
				data:'',
				tabIndex:0,
				topIndex:-1,
				topdata:'',
				topIDF:false,
				Catalog:'',
				datalog:'',
				pages:1,
				opt_id:'',
				showloadMore:false,
				loadMoreText:"加载中...",
				descnum:0
			}
		},
		methods:{
			descMenu(t,i){
				this.tabIndex=i;
				this.descnum=t.num;
				this.opt_id=uni.getStorageSync('optId');
				this.showloadMore=true;
				// 排序请求数据
				this.data='';
				uni.request({
					url: 'http://appserver.wujie520.cn/thirdreturn/index/classgoods?class_id='+this.opt_id+'&page='+this.pages+'&is_sub=0&prent_id='+this.opt_id+'&sort_type='+this.descnum,
					method: 'GET',
					data: {},
					success: res => {
						console.log("desc数据:",res.data);
						this.data=res.data.goods_search_response.goods_list;
						this.showloadMore=false;
					},
					fail: () => {},
					complete: () => {}
				});
			},
			goodLm(i){
				uni.navigateTo({
					url: '../productmess/productmess?id='+i.goods_id
				});
			},
			topdatas(){
				uni.request({
					url: 'http://appserver.wujie520.cn/thirdreturn/index/classgoods?class_id='+this.topdata+'&page=1&is_sub=1&prent_id='+this.topdata+'&sort_type='+this.descnum,
					method: 'GET',
					data: {},
					success: res => {
							console.log('头部数据',res.data);
							this.data=res.data[1].goods_search_response.goods_list;
							this.Catalog=res.data[0].goods_opt_get_response.goods_opt_list;
							this.topIDF=!this.topIDF;
							uni.hideLoading();
					}
				});
			},
			catalog(i,n){
				this.topIndex=n;
				// 储存页面opt_id
				uni.setStorageSync('optId',i.opt_id);
				// 顶部点击请求数据
				uni.request({
					url: 'http://appserver.wujie520.cn/thirdreturn/index/classgoods?class_id='+i.opt_id+'&page='+this.pages+'&is_sub=0&prent_id='+i.opt_id+'&sort_type='+this.descnum,
					method: 'GET',
					data: {},
					success: res => {
						console.log("点击返回数据",res.data);
						this.data=res.data.goods_search_response.goods_list;
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		onLoad:function(e) {
			console.log(e);
			this.datalog=e.name;
			console.log('类别;',this.datalog);
			    console.log("opid:",e);
				this.topdata=e.opid;
				// 跳转页面请求数据
				uni.showLoading({
					title:'加载中..'
				})
					uni.request({
						url: 'http://appserver.wujie520.cn/thirdreturn/index/classgoods?class_id='+e.id+'&page=1&is_sub=0&prent_id='+e.id+'&sort_type='+this.descnum,
						method: 'GET',
						data: {},
						success: res => {
							console.log("下面数据:",res.data);
							this.data=res.data.goods_search_response.goods_list;
							console.log(this.data);
							uni.hideLoading()
						}
					});
					this.topIDF=false;
					this.topdatas();
				    console.log(this.topIDF)
		},
		//触底
		onReachBottom() {
			this.pages+=1;
			this.opt_id=uni.getStorageSync('optId');
			this.showloadMore=true;
			uni.request({
				url: 'http://appserver.wujie520.cn/thirdreturn/index/classgoods?class_id='+this.opt_id+'&page='+this.pages+'&is_sub=0&prent_id='+this.opt_id,
				method: 'GET',
				data: {},
				success: res => {
					console.log("触底数据:",res.data);
					for(var i=0;i<res.data.goods_search_response.goods_list.length;i++){
						this.data.push(res.data.goods_search_response.goods_list[i])
					}
					this.showloadMore=false;
				},
				fail: () => {},
				complete: () => {}
			});
		}
	}
</script>

<style lang="less">
	.goodListMess{
		.active{
			color:#FFCB00;
		}
		// 与scroll-x 有关
		.uni-swiper-tab {
			width: 100%;
			white-space: nowrap;
			height: 100upx;
			position: fixed;
			top: 0;
			z-index: 100;
			background-color: #FFFFFF;
		}
		.goodListMenu{
			width: 160upx;
			line-height: 100upx;
			height: 100upx;
			display: inline-block;
			text-align: center;
			font-size:32upx;
		}
		height: 2000upx;
		// position:relative;
		.topMargin{
			margin-top: 60upx;
		}
		.allTopMargin{
			margin-top: 160upx;
		}
		.topPosition{
			position: fixed;
			top: 100upx;
		}
		.NonePosition{
			position: fixed;
			top: 0;
		}
		.decscmenu{
			box-sizing: border-box;
			width: 100%;
			height: 60upx;
			display: flex;
			flex-direction:row;
			justify-content: space-between;
			align-items:center;
			background-color:#FFFFFF;
			padding: 0 8upx;
			.active{
				color:#FFCB00;
			}
			.descName{
				width: 33%;
				height:60upx;
				line-height:60upx;
				text-align: center;
				font-size:26upx;
			}
		}
		// 商品
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
					>text:first-child{
						width: 10%;
						height: 24upx;
						line-height: 24upx;
						color:#FFFFFF;
						background:linear-gradient(to right,#FE877B,#DC505D);
					}
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
						>text:first-child{
							color:#cccccc;
						}
						>text:nth-child(2){
							color:#b22222;
						}
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
	}
</style>
