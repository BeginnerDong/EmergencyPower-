<template>
	<view>
		<view class="myExtendTop white">
			<view class="money">{{userInfoData.balance}}</view>
			<view class="yuan">总余额(元)</view>
			<view class="txBtn fs13" 
			@click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">提现</view>
		</view>
		<view class="f5bj timePx flexRowBetween fs13">
			<view class="item">
				<view class="flexCenter">
					开始时间<image class="arrw" src="../../static/images/assets-icon.png" mode=""></image>
				</view>
				<view class="flexCenter fs12 color9">
					<picker mode="date" :value='start' @change="changeStartTime">
						{{start==''?'请选择':start}} 
					</picker>
				</view>
			</view>
			<view class="item">
				<view class="flexCenter">
					结束时间<image class="arrw" src="../../static/images/assets-icon.png" mode=""></image>
				</view>
				<view class="flexCenter fs12 color9">
					<picker mode="date" :value='end' @change="changeEndTime">
						 {{end==''?'请选择':end}} 
					</picker>
				</view>
			</view>
		</view>
		<view class="orderNav flexRowBetween pdlr4 borderB1">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">收入明细</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">支出明细</view>
		</view>
		
		<view class="myRowBetween pdlr4" v-if="current==1">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="ll color6 flex">
					<view><image class="photo" :src="item.user?item.user.headImgUrl:''" ></image></view>
					<view class="photoName">
						<view class="fs13 color2">{{item.user?item.user.nickname:''}}</view>
						<view class="fs12">{{item.create_time}}</view>
					</view>
				</view>
				<view class="rr">
					<view>
						<view>购买商品</view>
						<view class="red">+{{item.count}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="myRowBetween pdlr4" v-if="current==2">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="ll color6 flex">
					
					<view class="photoName">
						<view class="fs13 color2">提现</view>
						<view class="fs12">{{item.create_time}}</view>
					</view>
				</view>
				<view class="rr">
					<view>
						<view class="red">{{item.count}}</view>
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
				current:1,
				orderOkData:[{},{},{}],
				searchItem:{
					type:2,
					status:['in',[0,1]],
					count:['>',0]
				},
				userInfoData:{},
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					pagesize: 10,
					is_page: true,
				},
				end:'',
				start:''
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self =  this;
			self.getUserInfoData();
			self.getMainData(true);
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
			
			changeStartTime(e){
				const self = this;
				self.start = e.detail.value;
				
				self.startTimestamp = self.$Utils.timeToTimestamp(self.start);
				if(self.startTimestamp&&self.endTimestamp){
					self.searchItem.create_time = ['between',[self.startTimestamp,self.endTimestamp]];
					self.getMainData(true)
				};
			},
			
			changeEndTime(e){
				const self = this;
				self.end = e.detail.value;
				
				self.endTimestamp = self.$Utils.timeToTimestamp(self.end);
				if(self.startTimestamp&&self.endTimestamp){
					self.searchItem.create_time = ['between',[self.startTimestamp,self.endTimestamp]];
					self.getMainData(true)
				};
			},
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						self.searchItem.count = ['>',0]
					}else if(self.current==2){
						self.searchItem.count = ['<',0]
					};
					self.getMainData(true)
				}
			},
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getThirdToken'
				postData.searchItem.user_no = uni.getStorageSync('thirdInfo').user_no		
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];

					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.tokenFuncName = 'getThirdToken'
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'relation_user',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['nickname','headImgUrl']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log(self.mainData)
				};
				self.$apis.flowLogGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	.timePx{padding: 20rpx 4%;background: #F5F5F5;}
	.timePx .item{width: 50%;}
	.timePx .arrw{width: 20rpx;height: 12rpx;margin-left: 10rpx;}
	
	.orderNav .tt{width:50%;}
	
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 80rpx 4% 80rpx 4%;background: #0094DC;}
	.myExtendTop .money{font-size: 80rpx; font-weight: bold; line-height:88rpx;}
	.myExtendTop .yuan{ font-size:28rpx; padding:10rpx 0 30rpx 0;}
	.myExtendTop .txBtn{ width: 120rpx; height:50rpx;line-height: 50rpx; background: #fff; color: #0094DC; border-radius: 30rpx; margin: 0 auto;}
	
</style>

