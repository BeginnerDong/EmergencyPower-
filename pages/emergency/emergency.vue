<template>
	<view class="" >
		<view class="orderNav pdlr4 whiteBj flexRowBetween">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">静音发电</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">油机发电机</view>
		</view>
		<view class="pdtb20 mgb15"></view>
		<view class="mglr4" v-show="curr==1">
			<view class="rankingNav flexRowBetween fs13 color6 pdb15">
				<view class="item flexCenter">
					<view>销量</view>
					<view class="fs-down flexColumn mgl5">
						<view class="arrow top on"></view>
						<view class="arrow bottom"></view>
					</view>
				</view>
				<view class="item flexCenter">
					<view>价格</view>
					<view class="fs-down flexColumn mgl5">
						<view class="arrow top"></view>
						<view class="arrow bottom on"></view>
					</view>
				</view>
			</view>
			<view class="proRowList">
				<view class="item flexRowBetween" v-for="(item,index) in muteProDate" :key="index" @click="Router.navigateTo({route:{path:'/pages/emergencyDetail/emergencyDetail'}})">
					<view class="ll">
						<image src="../../static/images/yingji-img.png"></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
						<view class="fs10 color9">设备总功率：50KW</view>
						<view class="Bmny flexRowBetween">
							<view class="price fs14">56/小时</view>
							<view class="fs10 color6">销量:235</view>
						</view>
						
					</view>
				</view>
			</view>
		</view>
		
		<view v-show="curr==2">
			<view class="leftNav fs12 color9 center">
				<view class="item" :class="specsCurr==index?'on':''" @click="changeSpecsCurr(index)" v-for="(item,index) in leftNavData" :key="index">{{item}}</view>
			</view>
			<view class="R-proList">
				<view class="rankingNav flexRowBetween fs13 color6 pdb15">
					<view class="item flexCenter">
						<view>销量</view>
						<view class="fs-down flexColumn mgl5">
							<view class="arrow top on"></view>
							<view class="arrow bottom"></view>
						</view>
					</view>
					<view class="item flexCenter">
						<view>价格</view>
						<view class="fs-down flexColumn mgl5">
							<view class="arrow top"></view>
							<view class="arrow bottom on"></view>
						</view>
					</view>
				</view>
				<view class="proRowList">
					<view class="item flexRowBetween" v-for="(item,index) in muteProDate" :key="index" @click="Router.navigateTo({route:{path:'/pages/emergencyDetail/emergencyDetail'}})">
						<view class="ll">
							<image src="../../static/images/yingji-img.png"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
							<view class="fs10 color9">设备总功率：50KW</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs13">56/小时</view>
								<view class="fs10 color6">销量:235</view>
							</view>
						</view>
					</view>
				</view>
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
				curr:1,
				muteProDate:[{},{},{},{}],
				leftNavData:['2KW-4KW','5KW-6KW','9KW-12KW'],
				specsCurr:0
			}
		},
		onLoad(options) {
			const self = this;
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			changeSpecsCurr(index){
				const self = this;
				self.specsCurr = index
			},
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.orderNav{position: fixed;top: 0rpx;left: 0;right: 0; z-index: 22;}
	.orderNav .tt{width: 50%;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .rr{width: 66%;}
	
	.leftNav{width: 165rpx;line-height: 96rpx;position: fixed;top:110rpx;bottom: 0;left: 0;z-index: 1;background: #fff;}
	.leftNav .item{border-bottom: 1px solid #F5F5F5;}
	.leftNav .item.on{background: #0094DC; color: #fff;position: relative;}
	.leftNav .item.on:after{content: '';position: absolute; top: 50%;right:-10rpx;transform: translateY(-50%);border-bottom: 6px solid transparent;border-top: 6px solid transparent;border-left: 6px solid #0094DC;}
	
	.R-proList{margin-left:186rpx;margin-right: 4%;}
	.R-proList .proRowList .rr{width: 56%;}
</style>
