<template>
	<view>
		<view class="whiteBj">
			<view class="userHead white pdlr4">
				<view class="infor pdt20">
					<view class="left flex">
						<image class="photo" src="../../static/images/about-img.png" mode=""></image>
						<view style="width: 70%;">
							<view class="fs14 pdb5">昵称昵称昵称昵称</view>
							<view class="fs13">156230352356</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="userBox2 boxShaow radius10 whiteBj">
				<view class="tit flexRowBetween pdlr4 pdt15 pdb10 borderB1">
					<view>我的订单</view>
					<view class="more flexEnd fs12 color9" @click="Router.navigateTo({route:{path:'/pages/user_myorder/user_myorder'}})">查看<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="menu flexRowBetween color6 fs12 pdtb15 center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_myorder/user_myorder'}})">
						<image src="../../static/images/about-icon1.png"></image>
						<view>待支付</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_myorder/user_myorder'}})">
						<image src="../../static/images/about-icon2.png"></image>
						<view>待发货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_myorder/user_myorder'}})">
						<image src="../../static/images/about-icon3.png"></image>
						<view>待收货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_myorder/user_myorder'}})" >
						<image src="../../static/images/about-icon4.png"></image>
						<view>已完成</view>
					</view>
				</view>
			</view>
			
			<view class="myRowBetween fs13 mglr4 mgt25">
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})" >
					<view class="ll flex">
						<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
						<view class="">服务地址</view>
					</view>
					<view class="rr"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myCollect/myCollect'}})" >
					<view class="ll flex">
						<image class="icon" src="../../static/images/about-icon6.png" mode=""></image>
						<view class="">我的收藏</view>
					</view>
					<view class="rr"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" >
					<view class="ll flex">
						<image class="icon" src="../../static/images/about-icon7.png" mode=""></image>
						<view class="">联系客服</view>
					</view>
					<view class="rr"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/thirdParty-login/thirdParty-login'}})" >
					<view class="ll flex">
						<image class="icon" src="../../static/images/about-icon8.png" mode=""></image>
						<view class="">第三方入口</view>
					</view>
					<view class="rr"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/staff-login/staff-login'}})" >
					<view class="ll flex">
						<image class="icon" src="../../static/images/about-icon9.png" mode=""></image>
						<view class="">员工登录</view>
					</view>
					<view class="rr"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
				</view>
			</view>
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
	@import "../../assets/style/navbar.css";
	page{background: #F5F5F5;}
	.userHead{height: 340rpx;background: url(../../static/images/about-icon.png) no-repeat 0 0 /100% 100%;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;}
	
	.userBox2{margin: -140rpx 4% 10rpx 4% ;position: relative; z-index: 1; box-shadow: 0 10rpx 20rpx rgba(0,0,0,0.1);}
	.userBox2 .item{width: 25%;}
	.userBox2 .item image{width: 50rpx;height:50rpx;display: block;margin: 0 auto;margin-bottom: 20rpx;}
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
	
</style>
