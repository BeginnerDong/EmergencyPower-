<template>
	<view>
		<view class="whiteBj mglr4 pdb15 refOrder" v-for="(item,index) in mainData" :key="index">
			<view class="mgt15">
				<view class="proRowList">
					<view class="item" >
						<view class="flexRowBetween pdb10">
							<view class="fs12 color9">交易时间：{{item.create_time}}</view>
							<view class="fs12 red" v-if="item.order_step==1">退款中</view>
							<view class="fs12 red" v-if="item.order_step==2">已退款</view>
							<!-- <view class="fs12 red" v-if="item.accept==1&&item.transport_status!=2">进行中</view>
							<view class="fs12 red" v-if="item.transport_status==2&&item.isremark==0">已完成</view>
							<view class="fs12 red" v-if="item.isremark==1">已评价</view> -->
						</view>
						<view class="flexRowBetween pdb15">
							<view class="ll">
								<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
								item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''"></image>
							</view>
							<view class="rr">
								<view class="avoidOverflow2 fs13">{{item.orderItem&&item.orderItem[0]&&
								item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
								<view class="Bmny flexRowBetween">
									<view class="price fs14">{{item.price}}</view>
									<view class="fs10 color6">已付：{{item.service_price}}元</view>
								</view>
							</view>
						</view>
						<!-- <view class="flexEnd fs13">
							<view>共1件商品 合计：￥56</view>
						</view> -->
					</view>
				</view>
			</view>
				
			<view class="refCon">
				{{item.passage1}}
			</view>
		
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:[],
				searchItem:{
					order_step:['>',0]
				},
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
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
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
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
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].money = '';
							self.mainData[i].price = parseFloat(self.mainData[i].price)
							self.mainData[i].service_price = parseFloat(self.mainData[i].service_price)
						}
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	}
</script>

<style>
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	
	.proRowList .item{padding: 20rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 160rpx; height: 160rpx;}
	.proRowList .rr{width: 72%;height: 160rpx;}
	
	.refOrder{border-radius: 10rpx;}
	.refCon{margin: 0 20rpx;background-color: #f5f5f5;border-radius: 10rpx;padding: 30rpx;font-size: 24rpx;color: #222;}
</style>
