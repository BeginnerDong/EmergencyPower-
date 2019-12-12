<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flexRowBetween">
					<view class="ll">公司名称</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">所在地</view>
					<view class="rr">
						<view class="color9">请选择</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">常用联系人</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">联系人电话</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">联系人邮箱</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="mgr15" style="width: 25%;">营业执照上传</view>
					<view class="flex" style="width:75%;flex-wrap: wrap;">
						<view class="flexCenter upImg radius8"><image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
						<view class="flexCenter upImg radius8"><image src="../../static/images/home-banner.png" mode=""></image></view>
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
	
	.upImg{width: 200rpx; height: 230rpx;background: #F5F5F5;margin-right: 20rpx;}
	.upImg image{width: 100%;height: 100%;}
	.upImg .jiaBtn{width:50rpx;height: 50rpx;display: block;}
	
</style>

