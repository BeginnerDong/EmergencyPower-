<template>
	<view>
		<view class="editMoney mglr4 radius10 whiteBj">
			<view class="card flexRowBetween borderB1"  v-if="userInfoData.bank_card!=''" @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg'}})">
				<view class="flex">
					<view>到账银行卡</view>
					<view class="mgl25 fs13 color6">{{userInfoData.label&&userInfoData.label[0]?userInfoData.label[0].title:''}}
					({{userInfoData.bank_card}})</view>
				</view>
				<view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="card flexRowBetween borderB1"  v-else  @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg'}})">
				<view class="flex">
					<view>到账银行卡</view>
					<view class="mgl25 fs13 color6">去添加</view>
				</view>
				<view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="tixian">
				<view class="tit pdtb20">提现金额</view>
				<view class="input borderB1 mgb10 flex">
					<input type="text"  v-model="submitData.count"/>
				</view>
				<view class="flexRowBetween">
					<view class="fs12 color6">本次可提现金额{{userInfoData.balance}}元</view>
					<view class="fs13" @click="allCount">全部提现</view>
				</view>
				<view class="submitbtn">
					<button class="Wbtn" type="button" @click="Utils.stopMultiClick(submit)">提现</button>
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
				Utils:this.$Utils,
				mainData:{},
				userInfoData:{},
				submitData:{
					count:''
				},
				
			}
		},
		
		onLoad(options) {
			const self = this;
			//self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			allCount(){
				const self = this;
				self.submitData.count = self.userInfoData.balance
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(!self.userInfoData.bank_card){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请先绑定银行卡', 'none');
					return
				};
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.submitData',self.submitData)
				if (pass) {
					if(parseFloat(self.submitData.count)>parseFloat(self.userInfoData.balance)){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('余额不足', 'none');
						return
					};
					if(parseFloat(self.submitData.count)<=0){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请输入正确的金额', 'none');
						return
					};
					if(parseFloat(self.submitData.count)<parseFloat(self.limit)){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('最少提现'+self.limit+'元', 'none');
						return
					};
					self.flowLogAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入提现金额', 'none')
				};
			},
			
			flowLogAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken'
				postData.data = {
					count:-self.submitData.count,
					thirdapp_id:2,
					status:1,
					trade_info:'提现',
					type:2,
					account:1,
					withdraw:1,
					withdraw_status:0,
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem.user_no = uni.getStorageSync('thridInfo').user_no;
				postData.refreshToken = true;
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						self.userInfoData.bank_card = self.userInfoData.bank_card.substr(self.userInfoData.bank_card.length-4);
						self.limit = parseFloat(uni.getStorageSync('thirdApp').limit)
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
			},

			getMainData() {
				const self = this;
			
				const postData = {};
				
				postData.tokenFuncName = 'getThirdToken';
				
				postData.searchItem = {
					isdefault:1
				};
				postData.getAfter ={
					label:{			
						tableName:'Label',
						middleKey:'bank',
						key:'id',
						condition:'=',
						searchItem:{
							status:1,
						}
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.mainData.number = self.mainData.number.substr(self.mainData.number.length-4)			
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.cardGet(postData, callback);
			},

		},
	};
</script>


<style>
	page{ background: #f5f5f5;}
	
	.editMoney{margin: 120rpx 4%;}
	.editMoney .card{padding: 50rpx 7%;}
	.editMoney .tixian{padding: 0rpx 7%;}
	.editMoney .input{line-height: 100rpx;}
	.editMoney .input input{ font-size: 60rpx; line-height: 100rpx; display: block; width: 80%; height: 100rpx; box-sizing: border-box;}
	.editMoney .input::before{content: "￥"; font-size: 50rpx; margin-right: 10rpx;}
	.editMoney .submitbtn{padding:60rpx 0;}
</style> 
