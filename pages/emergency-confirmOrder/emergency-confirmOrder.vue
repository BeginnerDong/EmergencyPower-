<template>
	<view class="" >

		<view class="pdlr4 pdt15 pdb15 fs13 whiteBj">
			<view class="pdb10">服务地址</view>
			<view class="flexRowBetween"
			 v-if="addressData.name">
				<view class="" style="width: 90%;">
					<view class="flexRowBetween mgb5" style="align-items: flex-start;">
						<view style="width: 70%;">
							<view>{{addressData.passage2}}<text class="mgl15">{{addressData.name}}</text></view>
						</view>
						<view class="fs11 color9">设备总功率:{{addressData.passage1}}</view>
					</view>
					<view class="color6">{{addressData.city+addressData.detail}}</view>
				</view>
				<view class="flexEnd" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
			<view class="flexRowBetween" v-if="!addressData.name"
			 v-else>
				<view class="" style="width: 90%;">
					请选择服务地址
				</view>
				<view class="flexEnd" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="proRowList whiteBj">
			<view class="item pdtb15 mglr4 flexRowBetween" v-for="item in mainData">
				<view class="ll">
					<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''"></image>
				</view>
				<view class="rr">
					<view class="avoidOverflow2 fs13">{{item.product&&item.product.title?item.product.title:''}}</view>
					<view class="Bmny">
						<view class="price fs14">{{item.product&&item.product.title?item.product.price:''}}/小时</view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="myRowBetween fs13 whiteBj">
			<view class="item flexRowBetween">
				<view class="ll">首付给服务商金额</view>
				<view class="rr">
					<input type="text" v-model="firstMoney" @blur="check()" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">联系电话</view>
				<view class="rr">
					<input type="number" maxlength="11" v-model="phone" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">预约时间</view>
				<view class="rr">
					<hTimePicker sTime="0" cTime="24" interval="20" @changeTime="changeTime">
					  <view slot="pCon" class="changeTime">
					    {{book_time!=''?book_time:'请选择预约时间'}}
					  </view>
					</hTimePicker>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
				
			</view>
			<view class="item flexRowBetween" style="align-items: flex-start;">
				<view style="20%">时长</view>
				<view style="width: 80%;">
					<view class="flexEnd seltDay center fs13">
						<view class="btn" :class="currChoose==1?'on':''" @click="choose('1')">4小时</view>
						<view class="btn" :class="currChoose==2?'on':''" @click="choose('2')">8小时</view>
						<view class="btn" :class="currChoose==3?'on':''" @click="choose('3')">自定义</view>
					</view>
					<view class="flexEnd" v-if="currChoose==3">
						<view class="borderB1 pdb5 pdt5" style="width: 320rpx;">
							<input type="number" @blur="countTotalPrice()"  v-model="count" placeholder="输入时长"  placeholder-class="placeholder"/>
						</view>
					</view>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">发票信息</view>
				<view class="rr" @click="Router.navigateTo({route:{path:'/pages/invoiceInformation/invoiceInformation'}})">
					<view slot="pCon" class="color9">{{receipt>0?'开发票':'不开发票'}}</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexEnd">
				<view>超过{{mainData&&mainData[0]&&mainData[0].product?mainData[0].product.distance:''}}公里，1公里收取{{mainData&&mainData[0]&&mainData[0].product?mainData[0].product.distance_price:''}}元</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show" style="bottom: 120rpx;"></view>
		
		<!-- 合计弹框 -->
		 <view class="qjAlertCont costShow borderB1" v-show="is_costShow">
			<view class="mglr4 pdtb15 center fs15 borderB1">费用明细</view>
			<view class="infor fs13 pdb20">
				<view class="item flexRowBetween" v-for="(item,index) in costDate" @click="seltCost(index)">
					<span>{{item.title}}</span>
					<view>{{item.text}}</view>
				</view>
			</view>
		</view>
		
		<view class="underFix flexRowBetween">
			<view class="flex"  @click="costShow">
				合计：<view class="price">{{totalPrice}}</view>
				<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
			</view>
			
			<button class="btn" open-type="getUserInfo"  style="font-size:15px"
			@getuserinfo="Utils.stopMultiClick(submit)">立即购买</button>
		</view>
	</view>

</template>

<script>
	import hTimePicker from "@/components/h-timePicker/h-timePicker.vue";
	export default {
		components: { hTimePicker },
		data() {
			return {
				Utils:this.$Utils,
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				spaceDate:[{},{},{},{},{},{}],
				costDate:[
					
				],
				seltCur:false,
				is_costShow:false,
				addressData:{},
				mainData:[],
				totalPrice:0,
				pay:{},
				phone:'',
				firstMoney:'',
				count:0,
				currChoose:1,
				book_time :'',
				distance:0,
				distancePrice:0,
				receipt:0
			}
		},
		
		onLoad(options) {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			console.log(self.mainData)
			self.count = 4;
	
			
			//self.$Utils.loadAll(['getUserInfoData','getUserCouponData'], self);
			//self.$Utils.loadAll(['getLocation'], self);
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
				if(self.addressData.latitude){
					self.distance = self.$Utils.distance(parseFloat(self.addressData.latitude),parseFloat(self.addressData.longitude)
					,parseFloat(self.mainData[0].product.latitude),parseFloat(self.mainData[0].product.longitude));
				}
				self.countTotalPrice()
			}else{
				self.getAddressData()
			};
			
			if(uni.getStorageSync('receiptData')){
				self.receipt = 10;
			}else{
				self.receipt = 0
			}
		},
		
		
		methods: {
			
			check(){
				const self = this;
				if(parseFloat(self.firstMoney)>parseFloat(self.pay.wxPay.price)){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('首付款不能大于总金额','none')
					self.firstMoney = ''
				};
			},
			
			choose(curr){
				const self = this;
				if(self.currChoose!=curr){
					self.currChoose = curr;
					if(self.currChoose==1){
						self.count = 4
					}else if(self.currChoose==2){
						self.count = 8
					}
					self.countTotalPrice()
				}
			},
			
			getLocation(){
				const self = this;
				uni.getLocation({
				    type: 'wgs84',
				    success: function (res) {
						self.melongitude = res.longitude;
						self.melatitude = res.latitude;
				        console.log('当前位置的经度：' + res.longitude);
				        console.log('当前位置的纬度：' + res.latitude);
						if(self.addressData.latitude){
							self.distance = self.$Utils.distance(parseFloat(self.addressData.latitude),parseFloat(self.addressData.longitude)
							,parseFloat(self.melongitude.latitude),parseFloat(self.melongitude.longitude));
						}
						self.countTotalPrice()
				    }
				});
				self.$Utils.finishFunc('getLocation');
			},
			
			changeTime(e){
				const self = this;
				self.book_time = e
				console.log(e)
			},
			
			costShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_costShow = !self.is_costShow
			},
			
			seltCost(index){
				const self = this;
				self.seltCur = index
			},
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
						if(self.addressData.latitude){
							self.distance = self.$Utils.distance(parseFloat(self.addressData.latitude),parseFloat(self.addressData.longitude)
							,parseFloat(self.mainData[0].product.latitude),parseFloat(self.mainData[0].product.longitude));
						}
						self.countTotalPrice()
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			countTotalPrice() {
				const self = this;
				if(self.currChoose==3){
					if(self.count<8){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('自定义时长不能小于8小时','none')
						return
					}
				};
				self.totalPrice = 0;
				self.productPrice=0;
				self.distancePrice = 0;
				self.mainData[0].count = self.count;
				self.costDate = [];
				self.costDate.push({title:'单价',text:'￥'+self.mainData[0].product.price})
	
				for (var i = 0; i < self.mainData.length; i++) {
					self.productPrice += self.mainData[i].product.price * self.mainData[i].count;
				};
				
				self.costDate.push({title:'时长',text:self.count+'小时'},)
				
				if(self.distance>parseFloat(self.mainData[0].product.distance)){
					self.distancePrice = ((parseFloat(self.distance)-parseFloat(self.mainData[0].product.distance))*parseFloat(self.mainData[0].product.distance_price)).toFixed(2)
					self.costDate.push({title:'距离',text:self.distance+'公里'})
					self.costDate.push({title:'距离费用',text:'￥'+self.distancePrice})
				};
				self.totalPrice = (parseFloat(self.productPrice) + parseFloat(self.distancePrice)).toFixed(2);
				var wxPay = parseFloat(self.totalPrice);
				console.log('wxPay',wxPay)
				if (wxPay > 0) {
					self.pay.wxPay = {
						price: wxPay.toFixed(2),
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				console.log(self.pay)
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				if(self.currChoose==3){
					if(self.count<8){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('自定义时长不能小于8小时','none')
						return
					}
				};
				
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请选择服务地址','none')
				}else{
					if(self.phone==''){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请填写联系电话','none')
						return
					}
					if(self.firstMoney==''){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请输入首付金额','none')
						return
					}
					if(self.book_time==''){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请选择预约时间','none')
						return
					};
					var data = {};
					if(uni.getStorageSync('receiptData')){
						data  = self.$Utils.cloneForm(uni.getStorageSync('receiptData'))
					};
					
					data.phone=self.phone;
					data.book_time=self.book_time;
					data.distance_price=self.distancePrice;
					data.shop_no=self.mainData[0].product.user_no;
					data.price=self.pay.wxPay.price;
					data.service_price=self.firstMoney;
					
					var orderList = [
						{product_id:self.mainData[0].product_id,count:self.count,type:1,data:data,snap_address:self.addressData}
					];
					const callback = (user, res) => {
						self.addOrder(orderList)
					};
					self.$Utils.getAuthSetting(callback);
					
				}
			},
			
			addOrder(orderList) {
				const self = this;	
				if(self.orderId){
					self.goPay()
					return
				};
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {};
				if(uni.getStorageSync('receiptData')){
					postData.data  = self.$Utils.cloneForm(uni.getStorageSync('receiptData'))
				};
				postData.data.snap_address = self.addressData;
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						if(uni.getStorageSync('receiptData')){
							uni.removeStorageSync('receiptData')
						};
						self.goPay()
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
			
			goPay(order_id) {
				const self = this;	
				var shopMoney = parseFloat(self.firstMoney) - parseFloat(self.firstMoney)*(parseFloat(self.mainData[0].product.tax)/100)
				var platformMoney = parseFloat(self.firstMoney) - parseFloat(shopMoney);
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				postData.payAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							count:parseFloat(shopMoney).toFixed(2),
							thirdapp_id:2,
							status:1,
							trade_info:'首付款',
							type:2,
							account:1,
							user_no:self.mainData[0].product.user_no,
							relation_user:uni.getStorageSync('user_info').user_no
						},
					},
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
							relation_user:self.mainData[0].product.user_no
						},
					},
				];
				
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
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user_myorder/user_myorder'}})
									}, 1000);
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
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user_myorder/user_myorder'}})
							}, 1000);
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
	}
</script>

<style>
	@import "../../assets/style/proList.css";
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/confirmOrder.css";
	
	page{padding-bottom: 120rpx;background: #F5F5F5;}
	button{
		background: none;
		line-height: 1.5;
		margin: 0;
	}
	button::after{
		border: none;
	}
	.proRowList .item:last-child{border-bottom: 0;}
	
</style>
