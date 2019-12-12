<template>
	<view>
		<view class="pdlr4">
			<view class="specslist">
				<view class="item" v-for="(item,index) in specsDate" :key="index">
					<view>名称名称名称名称名称名称名称</view>
					<view class="flexRowBetween pdtb10">
						<view class="price">56.00</view>
						<view class="fs12 color6">库存：5620</view>
					</view>
					<view class="flexEnd">
						<view class="flex fs12 color6">
							<view class="flex mgr25"><image class="editIcon" src="../../static/images/address-icon3.png" mode="">删除</image></image></view>
							<view class="flex"><image class="editIcon" src="../../static/images/address-icon2.png" mode="">编辑</image></image></view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="fabubtn" @click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGl_specsEdit/thirdParty_serviceGl_specsEdit'}})">
			<image class="icon" src="../../static/images/service-icon.png" mode=""></image>
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
				specsDate:[{},{},{}]
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
	
	page{padding-bottom: 60rpx;}
	.specslist .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.editIcon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	
	/* 页面右侧固定按钮 */
	.fabubtn{ width: 120rpx;font-size: 28rpx; text-align: center; position: fixed; bottom: 20%;right: 20rpx; z-index: 66;}
	.fabubtn .icon{width: 90rpx; height: 90rpx;border-radius: 50%;display: block;margin:8rpx auto;box-shadow: 0 0 10rpx rgba(0,0,0,0.12);}
	
</style>

