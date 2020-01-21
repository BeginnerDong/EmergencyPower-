<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">姓名</view>
				<view class="rr">{{mainData.info?mainData.info.name:''}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">手机号码</view>
				<view class="rr">{{mainData.info?mainData.info.phone:''}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">账号</view>
				<view class="rr">{{mainData.login_name}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">密码</view>
				<view class="rr">***********</view>
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
				wx_info:{},
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('thirdInfo').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userGet(postData, callback);
			},
			
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	
	.myRowBetween .item{padding: 30rpx 4%;}
	
</style>
