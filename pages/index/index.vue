<template>
	<view>
		
		<view class="banner-box pdlr4">
			<view class="banner pdtb15">
				<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#0094dc">
					<block v-for="(item,index) in labelData" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item" class="slide-image" />
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>
		
		<view class="indHome pdlr4 flex fs14">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/emergency/emergency'}})">
				<image src="../../static/images/home-icon.png"></image>
				<view class="tit">应急发电</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/mechanics/mechanics'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">工程机械</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/operator/operator'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">操作人员</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		
		<view class="pdlr4">
			<view class="fs15 pdtb20 ftw">热门推荐</view>
			<view class="rankingNav flexRowBetween fs13 color6 pdb15">
				<view class="item flexCenter">
					<view>销量</view>
					<view class="fs-down flexColumn mgl5">
						<view class="arrow top on"></view>
						<view class="arrow bottom"></view>
					</view>
				</view>
				<view class="item flexCenter">
					<view>价格</view>
					<view class="fs-down flexColumn mgl5">
						<view class="arrow top"></view>
						<view class="arrow bottom on"></view>
					</view>
				</view>
			</view>
			<view class="proLis flexRowBetween">
				<view class="item-lis boxShaow" v-for="(item,index) in produtList" :key="index" @click="Router.navigateTo({route:{path:'/pages/pageDetail/pageDetail'}})">
					<view class="pic">
						<image src="../../static/images/home-img.png" alt="" />
					</view>
					<view class="infor">
						<view class="tit avoidOverflow fs13 pdb5">名称名称名称名称名称</view>
						<view class="flexRowBetween">
							<view class="price">56.00</view>
							<view class="fs10 color9">销量:562</view>
						</view>
						
					</view>
				</view>
			</view>
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">我的</view>
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
				wx_info:{},
				is_show:false,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				produtList: [
					{},{},{},{}
				]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	
	page {padding-bottom: 140rpx;}
	.swiper-box {height: 360rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item {width: 100%;}
	.swiper-box image {width: 100%;height: 100%;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 33.3%;text-align: center;padding-bottom: 15px; font-size: 24rpx; color: #222;display: flex; flex-direction: column;}
	.indHome .item image {width:110rpx;height: 110rpx; border-radius: 50%;margin: 0 auto 20rpx auto; }
	
</style>
