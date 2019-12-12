<template>
	<view>
		
		<view class="myRowBetween">
			<view class="item flexRowBetween">
				<view class="ll">姓名</view>
				<view class="rr">张瑞</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">手机号码</view>
				<view class="rr">15689561278</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">账号</view>
				<view class="rr">14554564545454</view>
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
	
	.myRowBetween .item{padding: 30rpx 4%;}
	
</style>
