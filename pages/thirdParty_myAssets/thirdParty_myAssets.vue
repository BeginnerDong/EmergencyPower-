<template>
	<view>
		<view class="myExtendTop white">
			<view class="money">5600</view>
			<view class="yuan">总余额(元)</view>
			<view class="txBtn fs13" @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">提现</view>
		</view>
		<view class="f5bj timePx flexRowBetween fs13">
			<view class="item">
				<view class="flexCenter">
					开始时间<image class="arrw" src="../../static/images/assets-icon.png" mode=""></image>
				</view>
				<view class="flexCenter fs12 color9">2019.02</view>
			</view>
			<view class="item">
				<view class="flexCenter">
					结束时间<image class="arrw" src="../../static/images/assets-icon.png" mode=""></image>
				</view>
				<view class="flexCenter fs12 color9">2019.10</view>
			</view>
		</view>
		<view class="orderNav flexRowBetween pdlr4 borderB1">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">收入明细</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">支出明细</view>
		</view>
		
		<view class="myRowBetween pdlr4" v-show="current==1">
			<view class="item flexRowBetween" v-for="(item,index) in orderOkData" :key="index">
				<view class="ll color6 flex">
					<view><image class="photo" src="../../static/images/assets-img.png" ></image></view>
					<view class="photoName">
						<view class="fs13 color2">张丹</view>
						<view class="fs12">201912.12</view>
					</view>
				</view>
				<view class="rr">
					<view>
						<view>购买商品</view>
						<view class="red">+200</view>
					</view>
				</view>
			</view>
		</view>
		<view class="myRowBetween pdlr4" v-show="current==2">
			<view class="item flexRowBetween" v-for="(item,index) in orderOkData" :key="index">
				<view class="ll color6 flex">
					<view><image class="photo" src="../../static/images/about-img.png" ></image></view>
					<view class="photoName">
						<view class="fs13 color2">李含</view>
						<view class="fs12">201912.12</view>
					</view>
				</view>
				<view class="rr">
					<view>
						<view>提现</view>
						<view class="red">-200</view>
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
				current:1,
				orderOkData:[{},{},{}]
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
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.timePx{padding: 20rpx 4%;background: #F5F5F5;}
	.timePx .item{width: 50%;}
	.timePx .arrw{width: 20rpx;height: 12rpx;margin-left: 10rpx;}
	
	.orderNav .tt{width:50%;}
	
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 80rpx 4% 80rpx 4%;background: #0094DC;}
	.myExtendTop .money{font-size: 80rpx; font-weight: bold; line-height:88rpx;}
	.myExtendTop .yuan{ font-size:28rpx; padding:10rpx 0 30rpx 0;}
	.myExtendTop .txBtn{ width: 120rpx; height:50rpx;line-height: 50rpx; background: #fff; color: #0094DC; border-radius: 30rpx; margin: 0 auto;}
	
</style>

