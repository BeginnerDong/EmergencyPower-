<template>
	<view>
		<view class="orderNav flex fs14 whiteBj mgb10 color6">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">待接单</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">已接单</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">已完成</view>
		</view>
		<view class="pdtb25"></view>
		<view class="pdlr4">
			<view class="proRowList">
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">待接单</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56</view>
								<view class="fs10 color6">×1</view>
							</view>
						</view>
					</view>
					<div class="underBtn flexEnd pdb5 pdt15">
						<span class="Bbtn gary">拒绝接单</span>
						<span class="Bbtn"  @click="jiedanShow">接单</span>
					</div>
				</view>
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">已完成</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56</view>
								<view class="fs10 color6">×1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdtb15">共1件商品 合计:￥56</view>
				</view>
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">已接单</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56/小时</view>
								<view class="fs10 color6">×1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdtb15">共1件商品 合计:￥56</view>
					<view class="textList">
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">时间</view>
							<view class="text fs13">2019.12.11</view>
						</view>
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">地点</view>
							<view class="text fs13">陕西省西安市雁塔区高新大都荟</view>
						</view>
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">规格</view>
							<view class="text fs13">挖掘机·全包</view>
						</view>
					</view>
					<div class="underBtn flexEnd pdb5 pdt10">
						<span class="Bbtn">完成</span>
					</div>
				</view>
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">已完成</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">56/小时</view>
								<view class="fs10 color6">×1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdtb15">共1件商品 合计:￥56</view>
					<view class="textList">
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">时间</view>
							<view class="text fs13">2019.12.11</view>
						</view>
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">地点</view>
							<view class="text fs13">陕西省西安市雁塔区高新大都荟</view>
						</view>
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">规格</view>
							<view class="text fs13">挖掘机·全包</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="jiedanShow radius10" v-show="is_jiedanShow">
			<view class="closebtn fs20 color9" @click="jiedanShow">×</view>
			<view class="item flex">
				<view class="name fs12">时间</view>
				<view class="text fs13">2019/12/12</view>
			</view>
			<view class="item flex">
				<view class="name fs12">地点</view>
				<view class="text fs13">陕西省西安市雁塔区大都荟</view>
			</view>
			<view class="item flex">
				<view class="name fs12">规格</view>
				<view class="text fs13 flex"><view class="lable">挖掘机·全包</view></view>
			</view>
			<view class="submitbtn pdt30 pdb25">
				<button class="btn" type="button" @click="toast1Tap">确认接单</button>
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
				current:1,
				is_jiedanShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
			},
			jiedanShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_jiedanShow = !self.is_jiedanShow
			},
			toast1Tap() {
				uni.showToast({
					title: "接单成功"
				})
				const self = this;
				self.is_show = !self.is_show
				self.is_jiedanShow = !self.is_jiedanShow
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
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	.orderNav{position: fixed;top: 0rpx;left: 0;right: 0; z-index: 22;}
	.orderNav .tt{width: 25%;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 160rpx; height: 160rpx;}
	.proRowList .rr{width: 72%;height: 160rpx;}
	.give-input{width: 200rpx;}
	.give-input input{width: 100%;text-align: right;font-size: 26rpx;}
	.underBtn .Bbtn{display: block; width: 140rpx;text-align: center; line-height: 48rpx; border:1px solid #0094DC; color: #0094DC;text-align: center; margin-left: 30rpx; border-radius: 8rpx; font-size: 26rpx;}
	.underBtn .Bbtn.gary{border: 1px solid #b6b6b6;color: #666;}
	
	.textList .name{width: 15%;}
	.textList .text{width: 85%;}
	
	.jiedanShow{width: 90%;box-sizing: border-box;position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%);background: #fff;z-index: 60;padding: 20rpx 4% 40rpx 4%;}
	.jiedanShow .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.jiedanShow .item .name{width: 15%;color: #999;}
	.jiedanShow .item .text{width: 85%;}
	.jiedanShow .lable{padding: 0 10rpx;background: #F5F5F5;}
</style>

