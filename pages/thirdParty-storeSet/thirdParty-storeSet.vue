<template>
	<view>
		
		<view class="titems fs15 mglr4 center flexRowBetween">
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty-storeSet-personal/thirdParty-storeSet-personal'}})">
				<image class="icon" src="../../static/images/the-store-icon3.png" mode=""></image>
				<view class="pdt10">个人</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty-storeSet-enterprise/thirdParty-storeSet-enterprise'}})">
				<image class="icon" src="../../static/images/the-store-icon2.png" mode=""></image>
				<view class="pdt10">企业</view>
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
	.titems{padding: 280rpx 4%;}
	.titems .item{width: 50%;}
	.titems .item .icon{width: 140rpx;height: 140rpx;border-radius: 50%;display: block;margin: 0 auto;}
</style>
