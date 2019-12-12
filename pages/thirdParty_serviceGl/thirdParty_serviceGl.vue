<template>
	<view>
		<view class="orderNav pdlr4 whiteBj flexRowBetween color6">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">应急发电</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">工程器械</view>
			<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">操作人员</view>
		</view>
		<view class="pdtb20 mgb15"></view>
		<view class="pdlr4">
			<view class="proRowList" v-show="curr==1">
				<view class="item" v-for="(item,index) in seviceDate" :key="index">
					<view class="flexRowBetween pdb10 fs12">
						<view class="color9">2019.12.11</view>
						<view class="color6">状态：<text class="state pubColor">已发布</text></view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56</view>
								<view class="color6 fs12">库存：5000</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdb5 pdt15">
						<view class="flex fs12 color6">
							<view class="flex mgr25"><image class="editIcon" src="../../static/images/address-icon3.png" mode="">删除</image></image></view>
							<view class="flex"><image class="editIcon" src="../../static/images/address-icon2.png" mode="">编辑</image></image></view>
						</view>
					</view>
					
				</view>
			</view>
			<view class="proRowList" v-show="curr==2">
				<view class="item" v-for="(item,index) in seviceDate" :key="index">
					<view class="flexRowBetween pdb10 fs12">
						<view class="color9">2019.12.11</view>
						<view class="color6">状态：<text class="state pubColor">已发布</text></view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/details-img2.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56</view>
								<view class="color6 fs12">库存：5000</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdb5 pdt15">
						<view class="flex fs12 color6">
							<view class="flex mgr25"><image class="editIcon" src="../../static/images/address-icon3.png" mode="">删除</image></image></view>
							<view class="flex"><image class="editIcon" src="../../static/images/address-icon2.png" mode="">编辑</image></image></view>
						</view>
						<div class="underBtn flexEnd">
							<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGl_specsAdd/thirdParty_serviceGl_specsAdd'}})">添加规格</span>
						</div>
					</view>
					
				</view>
			</view>
			<view class="proRowList" v-show="curr==3">
				<view class="item" v-for="(item,index) in seviceDate" :key="index">
					<view class="flexRowBetween pdb10 fs12">
						<view class="color9">2019.12.11</view>
						<view class="color6">状态：<text class="state pubColor">已发布</text></view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/home-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56</view>
								<view class="color6 fs12">库存：5000</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdb5 pdt15">
						<view class="flex fs12 color6">
							<view class="flex mgr25"><image class="editIcon" src="../../static/images/address-icon3.png" mode="">删除</image></image></view>
							<view class="flex"><image class="editIcon" src="../../static/images/address-icon2.png" mode="">编辑</image></image></view>
						</view>
					</view>
					
				</view>
			</view>
			
			
		</view>
		
		<view class="fabubtn"  @click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGlAdd/thirdParty_serviceGlAdd'}})">
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
				curr:1,
				seviceDate:[{},{}]
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	.orderNav{position: fixed;top: 0rpx;left: 0;right: 0; z-index: 22;}
	.orderNav .tt{width: 33.3%;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 180rpx; height: 180rpx;}
	.proRowList .rr{width: 70%;height: 160rpx;}
	
	.give-input{width: 200rpx;}
	.give-input input{width: 100%;text-align: right;font-size: 26rpx;}
	.underBtn .Bbtn{display: block; width: 120rpx;text-align: center; line-height: 50rpx;text-align: center; margin-left: 30rpx; border-radius: 8rpx; font-size: 24rpx;background: #0094DC; color: #fff;}
	
	.editIcon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	
	/* 页面右侧固定按钮 */
	.fabubtn{ width: 120rpx;font-size: 28rpx; text-align: center; position: fixed; bottom: 20%;right: 20rpx; z-index: 66;}
	.fabubtn .icon{width: 90rpx; height: 90rpx;border-radius: 50%;display: block;margin:8rpx auto;box-shadow: 0 0 10rpx rgba(0,0,0,0.12);}
	
</style>

