<template>
	<view>
		<view class="myRowBetween fs13">
			<view class="item flexRowBetween">
				<view class="ll">规格名称</view>
				<view class="rr"><input type="text" placeholder="请输入" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">价格</view>
				<view class="rr"><input type="text" placeholder="请输入" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">库存</view>
				<view class="rr"><input type="text" placeholder="请输入" placeholder-class="placeholder"></view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 400rpx;">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGl_specsAdd/thirdParty_serviceGl_specsAdd'}})">确定</button>
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
		
		onLoad(options) {
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
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	
	.myRowBetween .item{padding: 30rpx 4%;}
</style>

