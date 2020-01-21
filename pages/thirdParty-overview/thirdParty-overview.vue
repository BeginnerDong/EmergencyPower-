<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flexRowBetween">
					<view class="ll">开店时长</view>
					<view class="rr color6">{{time}}</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">诚信分</view>
					<view class="rr color6">{{mainData.faith}}</view>
				</view>
				<view class="item">
					<view class="mgb10">保障金</view>
					<view class="bzjMny flexRowBetween center">
						<view class="child">
							<view class="cont flexCenter one">
								<view>
									<view class="name">{{productData[0]?productData[0].title:''}}</view>
									<view class="pric">￥{{productData[0]?productData[0].price:''}}</view>
								</view>
							</view>
							<view class="mgt15 flexCenter"  @click="changeCurr('0')" v-if="productData[0]&&productData[0].order&&productData[0].order.length==0">
								<image class="seltIcon" 
								:src="curr==0?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>
							</view>
							<view class="mgt15 flexCenter" style="color: #0094DC;" v-if="productData[0]&&productData[0].order&&productData[0].order.length>0">
								已交 
							</view>
						</view>
						<view class="child">
							<view class="cont flexCenter two">
								<view>
									<view class="name">{{productData[1]?productData[1].title:''}}</view>
									<view class="pric">￥{{productData[1]?productData[1].price:''}}</view>
								</view>
							</view>
							<view class="mgt15 flexCenter"  @click="changeCurr('1')" v-if="productData[1]&&productData[1].order&&productData[1].order.length==0">
								<image class="seltIcon" :src="curr==1?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>
							</view>
							<view class="mgt15 flexCenter" style="color: #0094DC;" v-if="productData[1]&&productData[1].order&&productData[1].order.length>0">
								已交 
							</view>
						</view>
						<view class="child">
							<view class="cont flexCenter three">
								<view>
									<view class="name">{{productData[2]?productData[2].title:''}}</view>
									<view class="pric">￥{{productData[2]?productData[2].price:''}}</view>
								</view>
							</view>
							<view class="mgt15 flexCenter"  @click="changeCurr('2')" v-if="productData[2]&&productData[2].order&&productData[2].order.length==0">
								<image class="seltIcon" :src="curr==2?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>
							</view>
							<view class="mgt15 flexCenter" style="color: #0094DC;" v-if="productData[2]&&productData[2].order&&productData[2].order.length>0">
								已交 
							</view>
						</view>
					</view>
				</view>
			</view>
			
			
			<view class="submitbtn" style="margin-top: 160rpx;" v-if="curr>=0">
				<button class="btn" type="button"   @click="Utils.stopMultiClick(submit)">支付</button>
			</view>
			
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Utils:this.$Utils,
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				curr:-1,
				productData:[],
				pay:{},
				mainData:{},
				time:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData','getProductData'], self);
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
				var now = Date.parse(new Date())/1000;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('thirdInfo').user_no
				};

				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.mainData.faith = parseInt(self.mainData.faith)?parseInt(self.mainData.faith):0
						self.sec = now - self.$Utils.timeToTimestamp(self.mainData.create_time);
						console.log(self.$Utils.timeToTimestamp(self.mainData.create_time))
						console.log(now)
						
						self.time = self.$Utils.secondTo(self.sec)
						console.log(self.time)
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getProductData() {
				const self = this;
				const postData = {};
				self.productData = [];
				postData.searchItem = {
					type:2
				};
				postData.order = {
					listorder:'desc'
				};
				postData.getAfter = {
					order:{
						token:uni.getStorageSync('thirdToken'),
						tableName:'Order',
						middleKey:'id',
						key:'product_id',
						searchItem:{
							status:1,
							pay_status:1,
							type:2
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productData.push.apply(self.productData,res.info.data)
					} 
					console.log('self.productData', self.productData)
					self.$Utils.finishFunc('getProductData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			submit(index){
				const self = this;
				uni.setStorageSync('canClick',false);
				var data = {};

				var orderList = [
				
				];
				
				self.pay = {
					wxPay:{
						price:self.productData[self.curr].price
					}
				};
				var data = {
					price:self.pay.wxPay.price,
					category_id:self.productData[self.curr].category_id
				}
				orderList.push(
					{product_id:self.productData[self.curr].id,count:1,type:2,data:data},
				)
				self.addOrder(orderList)	
			},
			
			addOrder(orderList) {
				const self = this;	
				if(self.orderId){
					 self.payNow();
					 return
				};
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.tokenFuncName = 'getThirdToken';
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.payNow()
					} else {		
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			
			
			payNow(order_id) {
				const self = this;	
				
				const postData = self.$Utils.cloneForm(self.pay);
				postData.tokenFuncName = 'getThirdToken',
				postData.searchItem = {
					id: self.orderId
				};	
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									self.curr = -1;
									self.getProductData()
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							self.curr = -1;
							self.getProductData()
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.myRowBetween .item{padding: 30rpx 4%; align-items: flex-start;}
	.seltIcon{width: 36rpx;height: 36rpx;}
	.bzjMny .child{width:220rpx;}
	.bzjMny .child .cont{width: 100%;height: 160rpx;border-radius: 10rpx;color: #fff;}
	.bzjMny .child .name{font-size: 26rpx;margin-bottom: 10rpx;}
	.bzjMny .child .pric{font-size: 30rpx;}
	.bzjMny .child .cont.one{background: linear-gradient( #51cc5f,#25c42c)}
	.bzjMny .child .cont.two{background: linear-gradient( #6989ff,#3581ff)}
	.bzjMny .child .cont.three{background: linear-gradient( #ff9d4e,#ff781d)}
</style>

