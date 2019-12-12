<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flexRowBetween">
					<view class="ll">商品名称</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">选择类别</view>
					<view class="rr">
						<view class="color9">请选择</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">库存</view>
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
				<view class="item">
					<view class="mgr15">详情介绍</view>
					<view>
						<textarea style="height: 200rpx;border: 0;padding:20rpx 0;" value="" placeholder="请输入" />
					</view>
					<view class="flex upPicBox xq-upImg">
						<view class="list">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
						<view class="list">
							<image class="pic" src="../../static/images/details-img.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="item flex">
					<view style="width: 20%;">商品主图</view>
					<view class="flex upPicBox" style="width: 80%;">
						<view class="list">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
						<view class="list">
							<image class="pic" src="../../static/images/the-order-img.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="item flex">
					<view style="width: 20%;">轮播图</view>
					<view class="flex upPicBox lunbo-upImg" style="width: 80%;">
						<view class="list">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
						<view class="list">
							<image class="pic" src="../../static/images/details-img.png" mode=""></image>
						</view>
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
	
	.upPicBox{flex-wrap: wrap;}
	.upPicBox .list{width: 180rpx;height:180rpx;margin:0 20rpx 20rpx 0;border-radius: 8rpx;position: relative;background: #F5F5F5;}
	.upPicBox .list image{width: 100%;height: 100%;display: block;}
	.upPicBox .list image.jiaBtn{width:50rpx;height: 50rpx;display: block;position:absolute;top:50%;left: 50%;transform: translate(-50%,-50%);}
	
	.xq-upImg .list{width: 200rpx; height: 140rpx;}
	.upImg{width: 200rpx; height: 230rpx;background: #F5F5F5;}
	
	.lunbo-upImg .list{width: 300rpx;height: 152rpx;}
	
</style>

