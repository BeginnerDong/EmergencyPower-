<template>
	<view class="">
		<view class="myRowBetween mglr4">
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">门店名称</view>
				<view class="rr fs13">
					<input type="text" placeholder="请输入门店名称">
				</view>
			</view>	
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">联系人</view>
				<view class="rr fs13" >
					<input type="text" placeholder="请输入联系人">
				</view>
			</view>	
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">设备总功率</view>
				<view class="rr fs13" >
					<input type="number" placeholder="请输入设备总功率">
				</view>
			</view>	
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">所在地区</view>
				<view class="rr fs13 color6">
					请选择<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>	
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">详细地址</view>
				<view class="rr fs13" >
					<input type="text" placeholder="如街道、门牌号">
				</view>
			</view>	
			<view class="item flexRowBetween pdtb15 borderB1">
				<view class="ll">设为默认地址</view>
				<view class="rr">
					<switch style="transform:scale(0.75);" color="#0094DC" />
				</view>
			</view>	
			
			<view class="submitbtn"  style="margin-top: 200rpx;">
				<button class="btn" type="button"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">确定</button>
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
				is_show:false,
				score: '',
				wx_info: {}
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = ''
		},

		methods: {
			setDefault(){
				const self = this;
				self.is_show=!self.is_show
			},
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.mmIcon{width: 100rpx;height: 42rpx;display: inline-block;}
</style>
