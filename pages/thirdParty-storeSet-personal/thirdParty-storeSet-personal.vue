<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flex" style="align-items: center;">
					<view class="ll">个人头像</view>
					<view class="rr">
						<view  class="person-phtoto">
							<image src="../../static/images/the-store-icon3.png" mode=""></image>
						</view>
						<view><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">昵称</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入昵称" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">性别</view>
					<view class="rr">
						<view class="flex mgr20" @click="changeSex('1')">
							<image class="seltIcon" :src="currSex==1?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>男
						</view>
						<view class="flex" @click="changeSex('2')">
							<image class="seltIcon" :src="currSex==2?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>女
						</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">个人介绍</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">工作经历</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item">
					<view>身份证上传</view>
					<view class="flexCenter mgt10">
						<view class="flexCenter upImg"><image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
					</view>
					<view class="flexCenter mgt10">
						<view class="flexCenter upImg"><image src="../../static/images/home-banner.png" mode=""></image></view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 120rpx;">
				<button class="btn" type="button">确定</button>
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
				currSex:0
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeSex(currSex){
				const self = this;
				if(currSex!=self.currSex){
					self.currSex = currSex
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
	.person-phtoto{width: 60rpx;height: 60rpx;border-radius: 50%;background: #e1e1e1;overflow: hidden;}
	.person-phtoto image{width: 100%;height: 100%;display: block;border-radius: 50%;}
	
	.seltIcon{width: 36rpx;height: 36rpx; display: block;margin-right: 10rpx;}
	
	.upImg{width: 350rpx; height: 220rpx;background: #F5F5F5;}
	.upImg image{width: 100%;height: 100%;}
	
	.upImg .jiaBtn{width:50rpx;height: 50rpx;display: block;}
	
</style>

