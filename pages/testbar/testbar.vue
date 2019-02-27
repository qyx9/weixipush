<template>
	<view class="tsetbar">
		<!-- submenu -->
		<scroll-view id="tab-bar" class="uni-swiper-tab" scroll-x >
			<view v-for="(tab,index) in data" :key="index" :class="['swiper-tab-list',tabIndex==index ? 'active' : '']"
			 :id="index" :data-current="index" @click="tapTab(index)">{{tab.name}}</view>
		</scroll-view>
		<swiper :duration="500" :current='tabIndex' @change="changeTab" class="swiper-box">
			<swiper-item v-for="(item,index) in msg" :key="index">
				<scroll-view scroll-y class="list">
					<view class="fg" >{{item.name}}</view>
				</scroll-view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				tabIndex:0,
				data:[
					{
					  name:"推荐"
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
			}
		},
		methods:{
			tapTab(i){
				this.tabIndex=i;
			},
			changeTab(e){
				console.log(e);
				this.tabIndex=e.detail.current;
			}
		}
	}
</script>

<style>
	.tsetbar {
		display: flex;
		flex: 1;
		flex-direction: column;
		overflow: hidden;
		height: 100%;
	}
	.tsetbar{
		height: 1200upx;
	}
	.tsetbar .swiper-box {
		flex: 1;
		width: 100%;
		height: calc(100% - 100upx);
	}
	.tsetbar .swiper-tab-list{
		display: inline-block;
		width: 150upx;
		text-align: center;
	}
	.fg{
		width: 100%;
		height: 3000upx;
		background-color: #FE877B;
	}
	.active{
		color: #007AFF;
	}
	.tsetbar .list{
		width: 750upx;
		height: 1200upx;
	}
</style>
