<template>
	<view>
		<view class="myRowBetween">
				<view class="item flexRowBetween">
					<view class="ll">银行开户名</view>
					<view class="rr">
						<input type="text" v-model="submitData.bank_name" placeholder="请输入银行卡卡户名" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">选择银行：</view>
					<view class="rr">
						<picker @change="bindPickerChange" :value="index" :range="labelData" range-key="title">
							<div class="uni-input">{{labelData[index]&&labelData[index].title?labelData[index].title:'请选择'}}</div>
						</picker>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				
				<view class="item flexRowBetween">
					<view class="ll">银行卡卡号：</view>
					<view class="rr">
						<input type="number" v-model="submitData.bank_card" placeholder="请输入银行卡卡号" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">手机号码：</view>
					<view class="rr">
						<input type="number" v-model="submitData.bank_phone" placeholder="请输入联系电话" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">验证码：</view>
					<view class="rr">
						<view class="yam" style="width: 50%;">
							<input type="number" v-model="submitData.smsCode" style="text-align: center;" placeholder="请输入验证码" placeholder-class="placeholder">
						</view>
						
						<view class="yzmBtn pubColor mgl20" @click="sendCode()" v-if="!hasSend">{{text}}</view>
						<view class="yzmBtn pubColor mgl20"  v-else>{{text}}</view>
					</view>
				</view>
				
				<view class="submitbtn" style="margin-top: 160rpx;">
					<button class="btn" type="submit" @click="Utils.stopMultiClick(submit)">保存</button>
				</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				index:-1,
				labelData:[],
				Utils:this.$Utils,
				submitData:{
					bank_name:'',
					bank:'',
					bank_card:'',
					bank_phone:'',
					smsCode:''
				},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
			}
		},
		
		onShow(options) {
			const self = this;
			console.log(23243)
			self.$Utils.loadAll(['getLabelData'], self);
		},
		
		methods: {
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.bank_phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.bank_phone,
					
					}
				};
				var callback = function(res){
					if(res.solely_code==100000){
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
								
							}
							
						}, 1000);
					}else{
						self.$Utils.showToast('发送失败', 'none', 1000)
					};
				};
				self.$apis.codeGet(postData, callback);
			},
			
			bindPickerChange(e) {	
				const self=this;
				console.log('picker发送选择改变，携带值为', e.target.value)
				self.index = e.target.value;
				self.submitData.bank=self.labelData[self.index].id
			},
			
			getLabelData() {
				const self = this;	
				const postData = {};	
				postData.searchItem = {
					thirdapp_id:2,	
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['银行类型']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					create_time:'asc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
					}
					self.getMainData()
					console.log('self.labelData',self.labelData)
					
				};
				self.$apis.labelGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.userInfoUpdate();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			
			userInfoUpdate() {
				const self = this;
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.smsCode;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('thirdInfo').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(newObject);
				postData.smsAuth = {
					phone:self.submitData.bank_phone,						
					code:self.submitData.smsCode
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('修改成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('thirdInfo').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.bank_name = res.info.data[0].bank_name;
						self.submitData.bank_card = res.info.data[0].bank_card;
						self.submitData.bank_phone = res.info.data[0].bank_phone;
						self.submitData.bank = res.info.data[0].bank;
						for (var i = 0; i < self.labelData.length; i++) {
							if(self.labelData[i].id==res.info.data[0].bank){
								console.log(i)
								self.index = i;
								
							}
						}
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
		},
	};
</script>	
<style>
	@import "../../assets/style/user.css";
	@import "../../assets/style/editInfor.css";
	
	.myRowBetween .item{padding: 30rpx 4%;border-bottom: 1px solid #eee;}

</style> 
