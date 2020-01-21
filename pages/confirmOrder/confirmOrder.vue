<template>
	<view class="" >

		<view class="flexRowBetween cfmSetAdrs fs13 pdlr4 pdt15 pdb15 whiteBj" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
			<view class="yy-title">服务地址</view>
			<view class="yy-adrs flexEnd" v-if="addressData.city">
				<view class="cont">{{addressData.city+addressData.detail}}</view>
				<image class="arrowR" src="../../static/images/arrow-icon.png" alt=""/>
			</view>
			<view class="yy-adrs flexEnd"  v-else>
				<view class="cont">请选择</view>
				<image class="arrowR" src="../../static/images/arrow-icon.png" alt=""/>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="proRowList whiteBj">
			<view class="item pdtb15 mglr4 flexRowBetween">
				<view class="ll">
					<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''"></image>
				</view>
				<view class="rr">
					<view class="avoidOverflow2 fs13">{{mainData.title}}</view>
					<view class="Bmny">
						<view class="price fs14">{{mainData.price}}</view>
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
				<view class="ll">联系人</view>
				<view class="rr">
					<input type="text" v-model="name" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">联系电话</view>
				<view class="rr">
					<input type="text" maxlength="11" v-model="phone" placeholder="请输入" placeholder-class="placeholder" />
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
			<view class="item">
				<view>规格</view>
				<view class="space color6 flex center">
					<view class="tt" 
					v-for="(item,index) in array1" :key="index" :data-id="item.id"
					@click="Utils.inArray($event.currentTarget.dataset.id,choose_sku_item)!=-1?chooseSku(labelData[0].id,$event.currentTarget.dataset.id):''" 
					:class="Utils.inArray(item.id,choose_sku_item)==-1?'canNotChoose':(Utils.inArray(item.id,sku_item)!=-1?'on':'')">
						{{item.title}}
					</view>
				</view>
			</view>
			<view class="item flexRowBetween" style="align-items: flex-start;">
				<view style="20%">天</view>
				<view style="width: 80%;">
					<view class="flexEnd seltDay center fs13">
						<view class="btn" v-for="(item,index) in array2" :key="index" :data-id="item.id"
						@click="Utils.inArray($event.currentTarget.dataset.id,choose_sku_item)!=-1?chooseSku(labelData[1].id,$event.currentTarget.dataset.id):''"
						:class="Utils.inArray(item.id,choose_sku_item)==-1?'canNotChoose':(Utils.inArray(item.id,sku_item)!=-1?'on':'')">
							{{item.title}}
						</view>
					</view>
					<view class="flexEnd" v-if="skuData.behavior&&skuData.behavior==1">
						<view class="borderB1 pdb5 pdt5" style="width: 320rpx;">
							<input type="number" @blur="countTotalPrice()"  v-model="count" placeholder="输入天数"  placeholder-class="placeholder"/>
						</view>
					</view>
				</view>
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
		
		<view class="underFix flexRowBetween" style="z-index:999">
			<view class="flex"  @click="costShow">
				合计：<view class="price">{{totalPrice}}</view>
				<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
			</view>
			
			<button class="btn" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)" style="font-size:15px">立即购买</button>
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
				count:1,
				currChoose:1,
				book_time :'',
				distance:0,
				distancePrice:0,
				labelData:[],
				sku_item:[],
				choose_sku_item:[],
				skuData:{},
				array1:[],
				array2:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = uni.getStorageSync('payPro')[0].product_id;
			console.log(self.mainData)
			self.$Utils.loadAll(['getMainData'], self);
			//self.$Utils.loadAll(['getLocation'], self);
		},
		
		onShow() {
			const self = this;
			
		},
		
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				postData.getAfter = {
					sku:{
					    tableName:'Sku',
						middleKey:'product_no',
						key:'product_no',
						searchItem:{
							status:1,
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						for(var key in self.mainData.label){
							if(self.mainData.sku_array.indexOf(parseInt(key))!=-1){
							   self.labelData.push(self.mainData.label[key])
							};    
						};
						self.array1 = self.labelData[0].child;
						self.array2 = self.labelData[1].child;
						self.skuData = self.$Utils.cloneForm(self.mainData.sku[0]);
						if(self.skuData.behavior==1){
							self.count=8
						};
						self.choose_sku_item.push.apply(self.choose_sku_item,self.mainData.sku[0].sku_item);
						self.sku_item = self.skuData.sku_item;
						if(uni.getStorageSync('choosedAddressData')){
							self.addressData = uni.getStorageSync('choosedAddressData')
							if(self.addressData.latitude){
								self.distance = self.$Utils.distance(parseFloat(self.addressData.latitude),parseFloat(self.addressData.longitude)
								,parseFloat(self.mainData.latitude),parseFloat(self.mainData.longitude));
							}
							self.countTotalPrice()
						}else{
							self.getAddressData()
						};
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					console.log('self.mainData', self.mainData)
					console.log('self.labelData', self.labelData)
					console.log('self.skuData', self.skuData)
					console.log('self.choose_sku_item', self.choose_sku_item)
					console.log('self.sku_item', self.sku_item)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			chooseSku(parentid,id){
				const self = this;
			    self.skuData = {};
			    if(self.choose_sku_item.indexOf(id)==-1){
			      return;
			    };
			    self.choose_sku_item = [];
			    var sku = self.mainData.label[parentid];
			    for(var i=0;i<sku.child.length;i++){
			      if(self.sku_item.indexOf(sku.child[i].id)!=-1){
			        self.sku_item.splice(self.sku_item.indexOf(sku.child[i].id), 1);
			      };
			      self.choose_sku_item.push(sku.child[i].id);
			    };
			
			    
			    for (var i = 0; i < self.mainData.sku.length; i++) {
			      if(self.mainData.sku[i].sku_item.indexOf(parseInt(id))!=-1){
			        self.choose_sku_item.push.apply(self.choose_sku_item,self.mainData.sku[i].sku_item);  
			      };
			    };
			
			    for(var i=0;i<self.sku_item.length;i++){
			      if(self.choose_sku_item.indexOf(parseInt(self.sku_item[i]))==-1){
			        self.sku_item.splice(i, 1); 
			      };
			    };
			    self.sku_item.push(id);
			    for(var i=0;i<self.mainData.sku.length;i++){ 
			      if(JSON.stringify(self.mainData.sku[i].sku_item.sort())==JSON.stringify(self.sku_item.sort())){
			        self.id = self.mainData.sku[i].id;
			        self.skuData = self.$Utils.cloneForm(self.mainData.sku[i]);
					console.log('self.skuData',self.skuData)
					if(self.skuData.behavior==1){
						self.count=8
					}
			      };   
			    }; 
				self.countTotalPrice()
			},
			
			check(){
				const self = this;
				if(parseFloat(self.firstMoney)>parseFloat(self.pay.wxPay.price)){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('首付款不能大于总金额','none')
					self.firstMoney = ''
				};
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
							,parseFloat(self.mainData.latitude),parseFloat(self.mainData.longitude));
						}
						self.countTotalPrice()
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				self.distancePrice=0;
				self.productPrice=0;
				self.costDate = [];
				if(self.skuData.behavior==0){
					self.productPrice = self.skuData.price
					self.costDate.push({title:'套餐价',text:'￥'+self.productPrice})
				}else if(self.skuData.behavior==1){
					if(self.count<8){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('自定义时长不能小于8','none')
						return
					};
					self.costDate.push({title:'天数',text:self.count+'天'},);
					self.costDate.push({title:'单价',text:'￥'+self.skuData.price});
					self.productPrice = self.skuData.price*self.count
				};
				if(self.distance>parseFloat(self.mainData.distance)){
					self.distancePrice = ((parseFloat(self.distance)-parseFloat(self.mainData.distance))*parseFloat(self.mainData.distance_price)).toFixed(2);
					self.costDate.push({title:'距离',text:self.distance+'公里'})
					self.costDate.push({title:'距离费用',text:'￥'+self.distancePrice})
				};
				self.totalPrice = parseFloat(self.productPrice) + parseFloat(self.distancePrice);
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
				if(self.skuData.behavior==1){
					if(self.count<8){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('自定义时长不能小于8','none')
						return
					};
				};
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请选择服务地址','none')
				}else{
					if(self.phone==''){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请填写联系电话','none')
						return
					};
					if(self.name==''){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请填写联系人姓名','none')
						return
					};
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
					var data = {
						phone:self.phone,
						book_time:self.book_time,
						name:self.name,
						shop_no:self.mainData.user_no,
						price:self.pay.wxPay.price,
						service_price:self.firstMoney,
						
					}
					var orderList = [
						{sku_id:self.skuData.id,count:self.count,type:1,data:data,snap_address:self.addressData}
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
				postData.data.snap_address = self.addressData;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
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
				var shopMoney = parseFloat(self.firstMoney) - parseFloat(self.firstMoney)*(parseFloat(self.mainData.tax)/100)
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
							user_no:self.mainData.user_no,
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
							relation_user:self.mainData.user_no
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
	.space .tt.on{background: #0094DC;color: #fff;border: 1px solid #0094DC;}
	.space .tt.canNotChoose{border:1px dashed;} 
	.seltDay .btn.canNotChoose{border:1px dashed;}
</style>
