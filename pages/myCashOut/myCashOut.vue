<template>
	<view>
		<view class="editMoney mglr4 radius10 whiteBj">
			<view class="card flexRowBetween borderB1" @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg'}})">
				<view class="flex">
					<view>到账银行卡</view>
					<view class="mgl25 fs13 color6">中国银行(2356)</view>
				</view>
				<view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="tixian">
				<view class="tit pdtb20">提现金额</view>
				<view class="input borderB1 mgb10 flex">
					<input type="number" value="0"/>
				</view>
				<view class="flexRowBetween">
					<view class="fs12 color6">本次可提现金额200元</view>
					<view class="fs13">全部提现</view>
				</view>
				<view class="submitbtn">
					<button class="Wbtn" type="button">提现</button>
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
				is_show:false
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
	page{ background: #f5f5f5;}
	
	.editMoney{margin: 120rpx 4%;}
	.editMoney .card{padding: 50rpx 7%;}
	.editMoney .tixian{padding: 0rpx 7%;}
	.editMoney .input{line-height: 100rpx;}
	.editMoney .input input{ font-size: 60rpx; line-height: 100rpx; display: block; width: 80%; height: 100rpx; box-sizing: border-box;}
	.editMoney .input::before{content: "￥"; font-size: 50rpx; margin-right: 10rpx;}
	.editMoney .submitbtn{padding:60rpx 0;}
</style> 
