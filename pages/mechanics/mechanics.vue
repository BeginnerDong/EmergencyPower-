<template>
	<view>
		
		<view class="pdlr4">
			<view class="rankingNav flexRowBetween fs13 color6 pdtb15">
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
			<view class="proLis flexRowBetween">
				<view class="item-lis boxShaow" v-for="(item,index) in produtList" :key="index" @click="Router.navigateTo({route:{path:'/pages/pageDetail/pageDetail'}})">
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
				produtList: [{},{},{},{},{},{}]
			}
		},
		
		onLoad() {
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
	@import "../../assets/style/proList.css";
</style>
