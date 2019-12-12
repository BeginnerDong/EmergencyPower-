<template>
	<view>
		
		<view class="pingjia mglr4">
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
				pingjiaList:[{},{},{},{},{},{}]
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
	@import "../../assets/style/star.css";
	
	page{padding-bottom: 60rpx;}
	
</style>
