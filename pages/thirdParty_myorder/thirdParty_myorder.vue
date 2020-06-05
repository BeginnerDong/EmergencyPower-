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
				<view class="item" v-for="(item,index) in mainData">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：{{item.create_time}}</view>
						<view class="fs12 red" v-if="item.accept==0">待接单</view>
						<view class="fs12 red" v-if="item.accept==1&&item.transport_status!=2">已接单</view>
						<view class="fs12 red" v-if="item.transport_status==2">已完成</view>
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
								<!-- <view class="price fs14">{{item.orderItem&&item.orderItem[0]&&
							item.orderItem[0].snap_product?item.orderItem[0].snap_product.price:''}}</view> -->
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
								<!-- <view class="price fs14">{{item.orderItem&&item.orderItem[0]&&
							item.orderItem[0].snap_product.product?item.orderItem[0].snap_product.product.price:''}}</view> -->
							<view class="price fs14">{{item.price}}</view>
							<view class="fs10 color6">已付：{{item.service_price}}元</view>
							</view>
						</view>
					</view>
					<!-- <view class="flexEnd pdtb15">共1件商品 合计:￥56</view> -->
					<view class="textList" v-if="item.accept==1">
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">时间</view>
							<view class="text fs13">{{item.book_time}}</view>
						</view>
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">地点</view>
							<view class="text fs13">
							{{item.snap_address&&item.snap_address.city?item.snap_address.city+item.snap_address.detail:''}}
							</view>
						</view>
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">姓名</view>
							<view class="text fs13">
							{{item.snap_address&&item.snap_address.name?item.snap_address.name:''}}
							</view>
						</view>
						<view class="lis flex pdb5">
							<view class="name fs12 color6 mgr15">电话</view>
							<view class="text fs13">
							{{item&&item.phone?item.phone:''}}
							</view>
						</view>
						<view class="lis flex pdb5" v-if="item.sku_id>0">
							<view class="name fs12 color6 mgr15">规格</view>
							<view class="text fs13">{{item.orderItem&&item.orderItem[0]&&
							item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
						</view>
					</view>
					<div class="underBtn flexEnd pdb5 pdt10">
						
						<span class="Bbtn gary"  v-if="item.accept==0" @click="orderUpdate(item.id,'no')">拒绝接单</span>
						<span class="Bbtn" v-if="item.accept==0"  @click="jiedanShow(item.id,index)">接单</span>
					</div>
				</view>
			</view>
		</view>
	
		<view class="black-bj" v-show="is_show"></view>
		<view class="jiedanShow radius10" v-show="is_jiedanShow">
			<view class="closebtn fs20 color9" @click="jiedanShow">×</view>
			<view class="item flex">
				<view class="name fs12">时间</view>
				<view class="text fs13">{{mainData[index]?mainData[index].book_time:''}}</view>
			</view>
			<view class="item flex">
				<view class="name fs12">地点</view>
				<view class="text fs13">{{mainData[index]&&mainData[index].snap_address&&mainData[index].snap_address.city?mainData[index].snap_address.city+mainData[index].snap_address.detail:''}}</view>
			</view>
			<view class="item flex" v-if="mainData[index].sku_id>0">
				<view class="name fs12">规格</view>
				<view class="text fs13 flex"><view class="lable">{{mainData[index]&&mainData[index].orderItem&&mainData[index].orderItem[0]&&
				mainData[index].orderItem[0].snap_product?mainData[index].orderItem[0].snap_product.title:''}}</view></view>
			</view>
			<view class="submitbtn pdt30 pdb25">
				<button class="btn" type="button" @click="orderUpdate">确认接单</button>
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
				is_jiedanShow:false,
				mainData:[],
				searchItem:{
					type:1,
					user_type:0,
					pay_status:1
				},
				willId:-1,
				index:-1
			}
		},
		
		onLoad(options) {
			const self = this;
			self.type=options.type;
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
			
			orderUpdate(id,type) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.data = {
					accept:1
				};
				postData.searchItem = {
					id:self.willId,
					user_type:0
				};
				if(type&&type=='no'){
					postData.data.accept = -1;
					postData.searchItem.id = id;
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						self.is_show = false;
						self.is_jiedanShow = false;
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
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
						self.searchItem.transport_status!=2;
					}else if(self.current==3){
						self.searchItem.accept = 1;
						//self.searchItem.transport_status = 1
						self.searchItem.transport_status!=2;
					}else if(self.current==4){
						self.searchItem.accept = 1;
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			},
			
			jiedanShow(id,index){
				const self = this;
				self.willId = id;
				self.index = index;
				self.is_show = !self.is_show
				self.is_jiedanShow = !self.is_jiedanShow
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
				postData.tokenFuncName = 'getThirdToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				if(self.type==2){
					postData.searchItem.shop_no = 'U910872296194660'
				}else{
					postData.searchItem.shop_no = uni.getStorageSync('thirdInfo').user_no
				};
				//postData.shop_no = 
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1,
							user_type:0
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
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

