<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flexRowBetween">
					<view class="ll">开店时长</view>
					<view class="rr color6">1年2月</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">诚信分</view>
					<view class="rr color6">89</view>
				</view>
				<view class="item">
					<view class="mgb10">保障金</view>
					<view class="bzjMny flexRowBetween center">
						<view class="child" @click="changeCurr('1')">
							<view class="cont flexCenter one">
								<view>
									<view class="name">应急发电</view>
									<view class="pric">￥562</view>
								</view>
							</view>
							<view class="mgt15 flexCenter">
								<image class="seltIcon" :src="curr==1?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>
							</view>
						</view>
						<view class="child" @click="changeCurr('2')">
							<view class="cont flexCenter two">
								<view>
									<view class="name">工程机械</view>
									<view class="pric">￥592</view>
								</view>
							</view>
							<view class="mgt15 flexCenter">
								<image class="seltIcon" :src="curr==2?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>
							</view>
						</view>
						<view class="child" @click="changeCurr('3')">
							<view class="cont flexCenter three">
								<view>
									<view class="name">操作人员</view>
									<view class="pric">￥620</view>
								</view>
							</view>
							<view class="mgt15 flexCenter">
								<image class="seltIcon" :src="curr==3?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			
			<view class="submitbtn" style="margin-top: 160rpx;">
				<button class="btn" type="button">支付</button>
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
				curr:1
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
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
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.myRowBetween .item{padding: 30rpx 4%; align-items: flex-start;}
	.seltIcon{width: 36rpx;height: 36rpx;}
	.bzjMny .child{width:220rpx;}
	.bzjMny .child .cont{width: 100%;height: 160rpx;border-radius: 10rpx;color: #fff;}
	.bzjMny .child .name{font-size: 26rpx;margin-bottom: 10rpx;}
	.bzjMny .child .pric{font-size: 30rpx;}
	.bzjMny .child .cont.one{background: linear-gradient( #51cc5f,#25c42c)}
	.bzjMny .child .cont.two{background: linear-gradient( #6989ff,#3581ff)}
	.bzjMny .child .cont.three{background: linear-gradient( #ff9d4e,#ff781d)}
</style>

