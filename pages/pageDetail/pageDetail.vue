<template>
	<view>
		<view class="detailxqBan">
			<image src="../../static/images/details-img.png" mode=""></image>
		</view>
		<view class="detailTit mglr4 pdtb10">
			<view class="tit">标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
			<view class="fs13 color6 pdt5 pdb5">设备总功率：50KW</view>
			<view class="flexRowBetween">
				<view class="price">56.00/小时</view>
				<view class="fs10 color9">销量：12345</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="pdlr4">
			<view class="fs15 pdt15 pdb10 ftw">服务介绍</view>
			<view class="xqInfor">
				<view class="cont fs13 color6">
					<view>内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
					<view>
						<image src="../../static/images/details-img1.png" mode="widthFix" />
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="pingjia mglr4">
			<view class="flexRowBetween pdt15 pdb5">
				<view class="flex ftw fs15">评价</view>
				<view class="color9 fs12 flexEnd" @click="Router.navigateTo({route:{path:'/pages/pingjiaList/pingjiaList'}})">更多<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
			<view class="pjList pdtb15" v-for="(item,index) in pingjiaList" :key="index">
				<view class="flexRowBetween pdb10">
					<view class="flex">
						<image class="us-photo mgr10" src="../../static/images/assets-img.png" mode=""></image>
						<view class="color6">
							<view class="fs13">快乐的猫</view>
							<view class="fs10">2019.11.29</view>
						</view>
					</view>
					<view class="xing flexEnd">
						<view class="starBox mgr5">
							<image src="../../static/images/details-icon1.png" mode=""></image>
							<image src="../../static/images/details-icon1.png" mode=""></image>
							<image src="../../static/images/details-icon1.png" mode=""></image>
							<image src="../../static/images/details-icon1.png" mode=""></image>
							<image src="../../static/images/details-icon.png" mode=""></image>
						</view>
						<view class="fs12 color9">8分</view>
					</view>
				</view>
				<view class="fs13">发电机,发电机,质量放心,价格更优惠,专业销售各种规格型号,专业生产,顶极的服务,移动电站,品质卓越可信</view>
			</view>
		</view>
		
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left">
				<view class="ite" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/details-icon3.png" mode=""></image>
					<view>返回首页</view>
				</view>
				<view class="ite">
					<image src="../../static/images/details-icon5.png" mode=""></image>
					<view>收藏</view>
				</view>
				<view class="ite">
					<image src="../../static/images/details-icon4.png" mode=""></image>
					<view>客服</view>
				</view>
			</view>
			<view class="payBtn" @click="Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})">立即下单</view>
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
				pingjiaList:[{},{},{}]
			}
		},

		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},

		methods: {
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
			
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.orderGet(postData, callback);
			
			}
		}
	}
</script>

<style>
	@import "../../assets/style/detial.css";
	@import "../../assets/style/quill.css";
	@import "../../assets/style/star.css";
	
	page{padding-bottom: 120rpx;}
	
</style>
