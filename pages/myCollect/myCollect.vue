<template>
	<view class="" >
		<view class="orderNav pdlr4 whiteBj flexRowBetween color6">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">应急发电</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">工程器械</view>
			<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">操作人员</view>
		</view>
		<view class="pdtb20 mgb15"></view>
		<view class="mglr4" v-show="curr==1">
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
		
		<view class="mglr4" v-show="curr==2">
			<view class="proLis flexRowBetween">
				<view class="item-lis boxShaow" v-for="(item,index) in mechanicsList" :key="index" @click="Router.navigateTo({route:{path:'/pages/pageDetail/pageDetail'}})">
					<view class="pic">
						<image src="../../static/images/details-img2.png" alt="" />
					</view>
					<view class="infor">
						<view class="tit avoidOverflow fs13 pdb5">名称名称名称名称名称</view>
						<view class="flexRowBetween">
							<view class="price">56.00</view>
							<view class="fs10 color9">销量:562</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4" v-show="curr==3">
			<view class="proLis flexRowBetween">
				<view class="item-lis boxShaow" v-for="(item,index) in operatorList" :key="index" @click="Router.navigateTo({route:{path:'/pages/operatorDetail/operatorDetail'}})">
					<view class="pic">
						<image src="../../static/images/home-img.png" alt="" />
					</view>
					<view class="infor">
						<view class="tit avoidOverflow fs13 pdb5">名称名称名称名称名称</view>
						<view class="flexRowBetween">
							<view class="price">56.00</view>
							<view class="fs10 color9">销量:562</view>
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
				mechanicsList:[{},{},{},{},{}],
				operatorList:[{},{},{},{},{}]
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
	.orderNav .tt{width: 33.33%;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .rr{width: 66%;}
	
</style>
