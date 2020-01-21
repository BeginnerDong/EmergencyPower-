<template>
	<view>
		<view class="orderNav flex fs14 whiteBj mgb10 color6">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">待确认</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">进行中</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">已完成</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">已评价</view>
		</view>
		<view class="pdtb25"></view>
		<view class="pdlr4">
			<view class="proRowList">
				<view class="item" v-for="(item,index) in mainData">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：{{item.create_time}}</view>
						<view class="fs12 red" v-if="item.accept==0">待确认</view>
						<view class="fs12 red" v-if="item.accept==1&&item.transport_status!=2">进行中</view>
						<view class="fs12 red" v-if="item.transport_status==2&&item.isremark==0">已完成</view>
						<view class="fs12 red" v-if="item.isremark==1">已评价</view>
					</view>
					<view class="flexRowBetween pdb15" v-if="item.product_id>0">
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
					<view class="flexRowBetween pdb15" v-if="item.sku_id>0">
						<view class="ll">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product.product&&
							item.orderItem[0].snap_product.product.mainImg&&item.orderItem[0].snap_product.product.mainImg[0]?
							item.orderItem[0].snap_product.product.mainImg[0].url:''"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">{{item.orderItem&&item.orderItem[0]&&
							item.orderItem[0].snap_product.product?item.orderItem[0].snap_product.product.title:''}}</view>
							<view style="color:#666;font-size:12px">{{item.orderItem&&item.orderItem[0]&&
							item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">{{item.price}}</view>
								<view class="fs10 color6">已付：{{item.service_price}}元</view>
							</view>
						</view>
					</view>
					<view class="flexRowBetween pdb15 fs13" v-if="item.price>item.service_price">
						<view>付款给服务商</view>
						<view class="give-input"><input type="text" v-model="item.money" placeholder="请输入金额" placeholder-class="placeholder" /></view>
					</view>
					<div class="underBtn flexEnd pdb5 pdt15">
						<span class="Bbtn" v-if="item.accept==1&&item.transport_status!=2" @click="flowLogAdd(index,'all')">完成订单</span>
						<span class="Bbtn gary" v-if="item.price>item.service_price" @click="flowLogAdd(index)">确定付款</span>
						<span class="Bbtn gary" @click="orderUpdate(item.id)" v-if="item.accept==0">取消订单</span>
						<span class="Bbtn" v-if="item.transport_status==2&&item.isremark==0"  :data-id="item.id" 
						@click="Router.navigateTo({route:{path:'/pages/user_orderPingJia/user_orderPingJia?id='+$event.currentTarget.dataset.id}})">去评价</span>
						<span class="Bbtn" v-if="item.transport_status==2&&item.isremark==1">已评价</span>
					</div>
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
				current:1,
				searchItem:{
					type:1,
					pay_status:1
				},
				mainData:[],
				money:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			if(options.current){
				self.currentNew = options.current;
			}
		},
		
		onShow() {
			const self = this;
			if(self.currentNew&&self.currentNew!=1){
				self.change(self.currentNew)
			}else{
				self.mainData = [];
				self.getMainData(true)
			}
			
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
			
			
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						delete self.searchItem.transport_status;
						delete self.searchItem.isremark;
						delete self.searchItem.accept;
					}else if(self.current==2){
						self.searchItem.accept = 0
						delete self.searchItem.isremark;
					}else if(self.current==3){
						self.searchItem.accept = 1;
						delete self.searchItem.isremark;
						//self.searchItem.transport_status = 1
					}else if(self.current==4){
						self.searchItem.accept = 1;
						self.searchItem.transport_status = 2;
						delete self.searchItem.isremark;
					}else if(self.current==5){
						self.searchItem.isremark = 1
					}
					self.getMainData(true)
				}
			},
			
			flowLogAdd(index,type) {
				const self = this;
				uni.setStorageSync('canClick',false);
				if(type&&type=='all'){
					self.mainData[index].money = (parseFloat(self.mainData[index].price) - parseFloat(self.mainData[index].service_price)).toFixed(2)
				}
				if(parseFloat(self.mainData[index].money)>(parseFloat(self.mainData[index].price) - parseFloat(self.mainData[index].service_price))){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('金额不能大于当前剩余金额','none')
					return
				};
				if(self.mainData[index].product_id>0){
					var shopMoney = parseFloat(self.mainData[index].money) - parseFloat(self.mainData[index].money)*(parseFloat(self.mainData[index].orderItem[0].snap_product.tax)/100)
				};
				if(self.mainData[index].sku_id>0){
					var shopMoney = parseFloat(self.mainData[index].money) - parseFloat(self.mainData[index].money)*(parseFloat(self.mainData[index].orderItem[0].snap_product.product.tax)/100)
				};
				var platformMoney = parseFloat(self.mainData[index].money) - parseFloat(shopMoney);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					count:parseFloat(shopMoney).toFixed(2),
					thirdapp_id:2,
					status:1,
					trade_info:'付款',
					type:2,
					account:1,
					user_no:self.mainData[index].orderItem[0].snap_product.user_no,
					relation_user:uni.getStorageSync('user_info').user_no
				};
				postData.saveAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							count:parseFloat(platformMoney).toFixed(2),
							thirdapp_id:2,
							status:1,
							trade_info:'平台抽佣',
							type:2,
							account:1,
							user_no:'U910872296194660',
							relation_user:self.mainData[index].orderItem[0].snap_product.user_no
						},
					},
					{
						tableName: 'Order',
						FuncName: 'update',
						data: {
							service_price:parseFloat(self.mainData[index].service_price) + parseFloat(self.mainData[index].money)
						},
						searchItem:{
							id:self.mainData[index].id,
							user_type:0
						}
					},
				];
				if(type&&type=='all'){
					postData.saveAfter[1].data.transport_status=2
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('付款成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			orderUpdate(id) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					status:-1
				};
				postData.searchItem = {
					id:id,
					user_type:0
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
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
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	.orderNav{position:fixed;top: 0;right: 0;left: 0;z-index: 10;}
	.orderNav .tt{width: 20%;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 160rpx; height: 160rpx;}
	.proRowList .rr{width: 72%;height: 160rpx;}
	.give-input{width: 200rpx;}
	.give-input input{width: 100%;text-align: right;font-size: 26rpx;}
	.underBtn .Bbtn{display: block; width: 140rpx;text-align: center; line-height: 48rpx; border:1px solid #0094DC; color: #0094DC;text-align: center; margin-left: 30rpx; border-radius: 8rpx; font-size: 26rpx;}
	.underBtn .Bbtn.gary{border: 1px solid #b6b6b6;color: #666;}
</style>

