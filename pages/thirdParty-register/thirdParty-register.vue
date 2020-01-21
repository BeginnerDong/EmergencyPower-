<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flex">
					<view class="ll">手机号</view>
					<view class="rr">
						<input type="number" v-model="submitData.phone" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">验证码</view>
					<view class="rr">
						<view style="width: 50%;">
							<input type="number" v-model="submitData.smsCode" placeholder="输入验证码" placeholder-class="placeholder" style="text-align: center;" />
						</view>
						<view class="mgl20 pubColor" @click="sendCode()" v-if="!hasSend">{{text}}</view>
						<view class="mgl20 pubColor"  v-else>{{text}}</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">密码</view>
					<view class="rr">
						<input type="text" v-model="submitData.password" placeholder="确认输入密码" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">选择入驻原因</view>
					<view class="rr">
						<picker @change="reasonChange" :value="reasonNum" :range="reasonDate">
							<view class="color9">{{reasonDate[reasonNum]?reasonDate[reasonNum]:'请选择'}}</view>
						</picker>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">营业执照</view>
					<view class="rr">
						<view class="flexCenter upImg" v-if="submitData.license.length>0">
							<image :src="submitData.license&&submitData.license[0]?submitData.license[0].url:''" mode="">
								
							</image>
						</view>
						<view class="flexCenter upImg" v-if="submitData.license.length==0" @click="upLoadImg('license')">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">特种经营许可证</view>
					<view class="rr">
						<view class="flexCenter upImg" v-if="submitData.special.length>0">
							<image :src="submitData.special&&submitData.special[0]?submitData.special[0].url:''" mode="">
								
							</image>
						</view>
						<view class="flexCenter upImg" v-if="submitData.special.length==0" @click="upLoadImg('special')">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">其他资质</view>
					<view class="rr">
						<view class="flexCenter upImg" v-if="submitData.qualification.length>0">
							<image :src="submitData.qualification&&submitData.qualification[0]?submitData.qualification[0].url:''" mode="">
								
							</image>
						</view>
						<view class="flexCenter upImg" v-if="submitData.qualification.length==0" @click="upLoadImg('qualification')">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image>
						</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">往期经营报告</view>
					<view class="rr">
						<input type="text" v-model="submitData.report" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 80rpx;">
				<button class="btn" type="button" 
				@click="register()">确定</button>
				<view class="agreeSel flex pdt10">
					<view class="flex">
						<view class="selt" @click="xieyiSelt">
							<image v-if="!seltCurr" src="../../static/images/address-icon1.png" mode=""></image>
							<image v-if="seltCurr" src="../../static/images/address-icon.png" mode=""></image>
						</view>
						<view class="fs13" @click="xieyiShow">《入驻协议》</view>
					</view>
				</view>
			</view>
			
			<view class="xieyiAlert" v-if="is_xieyiShow">
				<view class="infor radius10">
					<view class="cont fs12">
						<view class="fs14 pdb20 center">{{mainData.title}}</view>
						<view class="content ql-editor" style="padding: 0;" v-html="mainData.content">
						</view>
					</view>
					<view class="closebtn fs24 color9" @click="xieyiShow">×</view>
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
				is_setShow:false,
				reasonNum:-1,
				reasonDate:['我要雇人办事','我要入驻赚钱'],
				is_xieyiShow:false,
				seltCurr:false,
				submitData:{
					phone:'',
					password:'',
					passage1:'',
					license:[],
					special:[],
					qualification:[],
					report:'',
					smsCode:''
				},
				mainData:{},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.phone,
					
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
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['入驻协议']],
						},
						middleKey: 'menu_id',
						key: 'id',
						condition: 'in',
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
			
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',
							type:'image'
						}, callback)
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
			register() {
				const self = this;
				
				const postData = {
					data:self.$Utils.cloneForm(self.submitData)					
				}
				postData.smsAuth = {						
					phone:self.submitData.phone,						
					code:self.submitData.smsCode,
				};
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.qualification;
				if (self.$Utils.checkComplete(newObject)) {	
					if(!self.seltCurr){
						self.$Utils.showToast('请查看并同意入驻协议', 'none');
						return
					};
					const callback = (res) => {
						if (res.solely_code == 100000) {
							self.$Utils.showToast('提交成功，等待后台审核...', 'none');
							setTimeout(function() {
								uni.navigateBack({
									delta:1
								})
							}, 1000);	
						} else {
							self.$Utils.showToast(res.msg, 'none');
						}
					}
					self.$apis.register(postData, callback);
				} else {
					self.$Utils.showToast('请补全信息', 'none');
				};
			},
			

			reasonChange(e) {
				const self = this;
				console.log('picker发送选择改变，携带值为', e.target.value)
				self.reasonNum = e.target.value;
				self.submitData.passage1 = self.reasonDate[self.reasonNum]
			},
			
			xieyiSelt(){
				const self = this;
				self.seltCurr= !self.seltCurr
			},
			
			xieyiShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_xieyiShow = !self.is_xieyiShow
				
			},

		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/xieyiAlert.css";
	page{padding-bottom: 60rpx;}
	.myRowBetween .item{padding: 30rpx 4%; align-items: flex-start;}
	
	.myRowBetween .item .ll{width: 30%;}
	.myRowBetween .item .rr{width: 70%;}
	
	.upImg{width: 180rpx; height: 140rpx;background: #F5F5F5;margin-left: 20rpx;}
	.upImg image{width: 100%;height: 100%;display: block;}
	.upImg image.jiaBtn{width:50rpx;height: 50rpx;display: block;}
	
	.alertBox{width: 80%; position: fixed;top: 50%;left:50%;transform: translate(-50%,-50%);box-sizing: border-box; z-index: 50;}
</style>

