<template>
	<view>
		<view class="userHead white pdlr4">
			<view class="infor">
				<view class="left flex">
					<image class="photo" src="../../static/images/about-img.png" mode=""></image>
					<view style="width: 70%;">
						<view class="fs14 pdb5">昵称昵称昵称昵称</view>
						<view class="fs13">156230352356</view>
					</view>
				</view>
			</view>
		</view>
		<view class="indHome flex fs14">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/thirdParty-overview/thirdParty-overview'}})">
				<image class="icon" src="../../static/images/disanfang-icon.png"></image>
				<view class="tit">概览</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty-storeSet/thirdParty-storeSet'}})">
				<image class="icon" src="../../static/images/disanfang-icon1.png"></image>
				<view class="tit">店铺设置</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGl/thirdParty_serviceGl'}})">
				<image class="icon" src="../../static/images/disanfang-icon2.png"></image>
				<view class="tit">服务管理</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myorder/thirdParty_myorder'}})">
				<image class="icon" src="../../static/images/disanfang-icon3.png"></image>
				<view class="tit">我的订单</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myAssets/thirdParty_myAssets'}})">
				<image class="icon" src="../../static/images/disanfang-icon4.png"></image>
				<view class="tit">我的资产</view>
			</view>
		</view>
			
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {

		},
	};
</script>

<style>
	.userHead{padding: 50rpx 4%;background: #0094DC;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;border-top: 1px solid #e1e1e1;}
	.indHome .item{width: 33.3%;text-align: center;padding:60rpx 0 50rpx 0;box-sizing: border-box;border-right: 1px solid #e1e1e1;border-bottom: 1px solid #e1e1e1;}
	.indHome .item:nth-of-type(3n){border-right: 0;box-sizing: border-box;}
	.indHome .item .icon{width:66rpx;height: 66rpx; margin: 0 auto 20rpx auto; }
	
	
</style>
