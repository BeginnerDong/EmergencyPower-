<template>
	<view>
		
		<view class="pdlr4 mgt15">
			<view class="proRowList">
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：{{mainData.create_time}}</view>
						<view class="fs12 red">进行中</view>
						<!-- <view class="fs12 red" v-if="item.accept==1&&item.transport_status!=2">进行中</view>
						<view class="fs12 red" v-if="item.transport_status==2&&item.isremark==0">已完成</view>
						<view class="fs12 red" v-if="item.isremark==1">已评价</view> -->
					</view>
					<view class="flexRowBetween pdb15">
						<view class="ll">
							<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
							mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">{{mainData.orderItem&&mainData.orderItem[0]&&
							mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.title:''}}</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">{{mainData.price}}</view>
								<view class="fs10 color6">已付：{{mainData.service_price}}元</view>
							</view>
						</view>
					</view>
					<!-- <view class="flexEnd fs13">
						<view>共1件商品 合计：￥56</view>
					</view> -->
				</view>
			</view>
		</view>
		
		<view class="color2 pdt5 pdb15 pdlr4">退款/售后原因</view>
		<view class="pdlr4 pdt-15">
			<textarea v-model="submitData.passage1" placeholder="请写出您的退款的原因,帮助您更快的审核通过~" class="refundCon"/>
		</view>
		
		<view class="btn" @click="orderUpdate()">提交</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				submitData:{
					passage1:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					order_step:1,
					passage1:self.submitData.passage1
				};
				postData.searchItem = {
					id:self.id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.Router.reLaunch({route:{path:'/pages/user/user'}})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
				};
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.mainData.money = '';
						self.mainData.price = parseFloat(self.mainData.price)
						self.mainData.service_price = parseFloat(self.mainData.service_price)
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 160rpx; height: 160rpx;}
	.proRowList .rr{width: 72%;height: 160rpx;}
	
	.refundCon{width: 690rpx;height: 300rpx;background-color: #fff;border-radius: 10rpx;}
	.btn{ height: 80rpx; text-align: center; line-height: 80rpx; color:#fff; background: #0094DC; border-radius: 10rpx;margin: 12% 10% 0;}
	.uni-input-placeholder{color: #999;}
	textarea{font-size: 24rpx;color: #999;padding: 30rpx;}
</style>
