<template>
	<view>
		<view class="whiteBj">
			<view class="userHead white pdlr4">
				<view class="infor pdt20">
					<view class="left flex">
						<image class="photo" src="../../static/images/about-img.png" mode=""></image>
						<view style="width: 70%;">
							<view class="fs14 pdb5">哈喽猫咪</view>
							<view class="fs13">156230352356</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="userBox2 boxShaow radius10 whiteBj">
				<view class="tit flexRowBetween pdlr4 pdt15 pdb10 borderB1">
					<view>我的订单</view>
					<view class="more flexEnd fs12 color9" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myorder/thirdParty_myorder'}})">查看<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="menu flexRowBetween color6 fs12 pdtb15 center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myorder/thirdParty_myorder'}})">
						<image src="../../static/images/employees-icon1.png"></image>
						<view>全部</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myorder/thirdParty_myorder'}})">
						<image src="../../static/images/employees-icon2.png"></image>
						<view>待接单</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myorder/thirdParty_myorder'}})">
						<image src="../../static/images/employees-icon3.png"></image>
						<view>已接单</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myorder/thirdParty_myorder'}})" >
						<image src="../../static/images/employees-icon4.png"></image>
						<view>已完成</view>
					</view>
				</view>
			</view>
			
			<view class="myRowBetween fs13 mglr4 mgt25">
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/staffUser-infor/staffUser-infor'}})" >
					<view class="ll flex">
						<image class="icon" src="../../static/images/employees-icon5.png" mode=""></image>
						<view class="">基础信息</view>
					</view>
					<view class="rr"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
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
	@import "../../assets/style/editInfor.css";
	
	.userHead{height: 290rpx;background: url(../../static/images/employees-icon.png) no-repeat 0 0 /100% 100%;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;}
	
	.userBox2{margin: -80rpx 4% 10rpx 4% ;position: relative; z-index: 1; box-shadow: 0 10rpx 20rpx rgba(0,0,0,0.1);}
	.userBox2 .item{width: 25%;}
	.userBox2 .item image{width: 50rpx;height:50rpx;display: block;margin: 0 auto;margin-bottom: 20rpx;}
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
	
</style>
