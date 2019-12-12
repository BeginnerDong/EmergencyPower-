<template>
	<view>
		<view class="orderNav flex fs14 whiteBj mgb10 color6">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">待确认</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">进行中</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">已完成</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">已评价</view>
		</view>
		<view class="pdtb25"></view>
		<view class="pdlr4">
			<view class="proRowList">
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">待确认</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56</view>
								<view class="fs10 color6">×1</view>
							</view>
						</view>
					</view>
					<div class="underBtn flexEnd pdb5 pdt15">
						<span class="Bbtn gary">取消订单</span>
						<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/user_myorder_confmOrder/user_myorder_confmOrder'}})">去支付</span>
					</div>
				</view>
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">进行中</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56</view>
								<view class="fs10 color6">×1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdtb15">共1件商品 合计:￥56</view>
					<view class="flexRowBetween pdb15 fs13">
						<view>付款给服务商</view>
						<view class="give-input"><input type="text" value="" placeholder="请输入金额" placeholder-class="placeholder" /></view>
					</view>
					<div class="underBtn flexEnd pdb5">
						<span class="Bbtn">确认</span>
					</div>
				</view>
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">已完成</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56/小时</view>
								<view class="fs10 color6">×1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdtb15">共1件商品 合计:￥56</view>
					<div class="underBtn flexEnd pdb5">
						<span class="Bbtn"  @click="Router.navigateTo({route:{path:'/pages/user_orderPingJia/user_orderPingJia'}})">去评价</span>
					</div>
				</view>
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">已评价</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56/小时</view>
								<view class="fs10 color6">×1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdtb15">共1件商品 合计:￥56</view>
					<view class="pdb10">
						<view class="pdlr4 pdt10 pdb10 f5bj radius8 fs12">科就回归考虑到双方各烘干机撒联合国何健飞道快乐十分个分类山沟加工费是发的是</view>
					</view>
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
				wx_info:{},
				is_show:false,
				current:1
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
			},
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	.orderNav{position:fixed;top: 0;right: 0;left: 0;z-index: 10;}
	.orderNav .tt{width: 20%;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 160rpx; height: 160rpx;}
	.proRowList .rr{width: 72%;height: 160rpx;}
	.give-input{width: 200rpx;}
	.give-input input{width: 100%;text-align: right;font-size: 26rpx;}
	.underBtn .Bbtn{display: block; width: 140rpx;text-align: center; line-height: 48rpx; border:1px solid #0094DC; color: #0094DC;text-align: center; margin-left: 30rpx; border-radius: 8rpx; font-size: 26rpx;}
	.underBtn .Bbtn.gary{border: 1px solid #b6b6b6;color: #666;}
</style>

