<template>
	<view>
		<view class="userHead white pdlr4">
			<view class="infor">
				<view class="left flex">
					<view class="photo" style="overflow: hidden;">
						<open-data type="userAvatarUrl"></open-data>
					</view>
					<view style="width: 70%;">
						<view class="fs14 pdb5"><open-data type="userNickName"></open-data></view>
						<view class="fs13">{{mainData.phone}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="indHome flex fs14">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/thirdParty-overview/thirdParty-overview'}})">
				<image class="icon" src="../../static/images/disanfang-icon.png"></image>
				<view class="tit">概览</view>
			</view>
			<view class="item" @click="submit">
				<image class="icon" src="../../static/images/disanfang-icon1.png"></image>
				<view class="tit">店铺设置</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGl/thirdParty_serviceGl'}})">
				<image class="icon" src="../../static/images/disanfang-icon2.png"></image>
				<view class="tit">服务管理</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myorder/thirdParty_myorder?type=1'}})">
				<image class="icon" src="../../static/images/disanfang-icon3.png"></image>
				<view class="tit">我的订单</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/thirdParty_myAssets/thirdParty_myAssets'}})">
				<image class="icon" src="../../static/images/disanfang-icon4.png"></image>
				<view class="tit">我的资产</view>
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
			
			submit(){
				const self = this;
				if(self.mainData.identity==0){
					self.Router.navigateTo({route:{path:'/pages/thirdParty-storeSet/thirdParty-storeSet'}})
				}else if(self.mainData.identity==1){
					self.Router.navigateTo({route:{path:'/pages/thirdParty-storeSet-enterprise/thirdParty-storeSet-enterprise'}})
				}else if(self.mainData.identity==2){
					self.Router.navigateTo({route:{path:'/pages/thirdParty-storeSet-personal/thirdParty-storeSet-personal'}})
				}
			},
			
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
				self.$apis.userInfoGet(postData, callback);
			},
		},
	};
</script>

<style>
	.userHead{padding: 50rpx 4%;background: #0094DC;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;border-top: 1px solid #e1e1e1;}
	.indHome .item{width: 33.3%;text-align: center;padding:60rpx 0 50rpx 0;box-sizing: border-box;border-right: 1px solid #e1e1e1;border-bottom: 1px solid #e1e1e1;}
	.indHome .item:nth-of-type(3n){border-right: 0;box-sizing: border-box;}
	.indHome .item .icon{width:66rpx;height: 66rpx; margin: 0 auto 20rpx auto; }
	
	
</style>
