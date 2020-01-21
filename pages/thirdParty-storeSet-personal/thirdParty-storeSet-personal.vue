<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flex" style="align-items: center;">
					<view class="ll">个人头像</view>
					<view class="rr">
						<view  class="person-phtoto" v-if="submitData.mainImg.length>0">
							<image :src="submitData.mainImg&&submitData.mainImg[0]?submitData.mainImg[0].url:''" mode=""></image>
						</view>
						<view  class="person-phtoto" v-if="submitData.mainImg.length==0" @click="upLoadImg('mainImg')">
							<image src="../../static/images/the-store-icon3.png" mode=""></image>
						</view>
						<view><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">昵称</view>
					<view class="rr">
						<input type="text" v-model="submitData.name" placeholder="请输入昵称" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">性别</view>
					<view class="rr">
						<view class="flex mgr20" @click="changeSex('1')">
							<image class="seltIcon" :src="currSex==1?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>男
						</view>
						<view class="flex" @click="changeSex('2')">
							<image class="seltIcon" :src="currSex==2?'../../static/images/address-icon.png':'../../static/images/address-icon1.png'" mode=""></image>女
						</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">个人介绍</view>
					<view class="rr">
						<input type="text" v-model="submitData.introduction" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">工作经历</view>
					<view class="rr">
						<input type="text" v-model="submitData.experience" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item">
					<view>身份证上传</view>
					
					<view class="flexCenter mgt10" v-for="item in submitData.id_img" v-if="submitData.id_img.length>0">
						<view class="flexCenter upImg">
							<image :src="item.url" mode=""></image>
						</view>
					</view>
					<view class="flexCenter mgt10" @click="upLoadImg('id_img')">
						<view class="flexCenter upImg"><image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 120rpx;">
				<button class="btn" type="button" @click="submit()">确定</button>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				currSex:-1,
				submitData:{
					mainImg:[],
					gender:'',
					introduction:'',
					experience:'',
					id_img:[],
					name:'',
					identity:2
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
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
			
			changeSex(currSex){
				const self = this;
				if(currSex!=self.currSex){
					self.currSex = currSex;
					self.submitData.gender = self.currSex
				}
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
						
						if(type=="mainImg"){
							self.submitData[type] = [];
						};
						self.submitData[type].push({url:res.info.url,type:'image'})
						 console.log('type',type)
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
							tokenFuncName: 'getThirdToken',
							type:'image'
						}, callback)
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('thirdInfo').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
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
						self.submitData.mainImg = self.mainData.mainImg
						self.submitData.name = self.mainData.name
						self.submitData.gender = self.mainData.gender
						self.currSex  = self.mainData.gender;
						self.submitData.introduction = self.mainData.introduction
						self.submitData.experience = self.mainData.experience
						self.submitData.id_img = self.mainData.id_img
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	
	.myRowBetween .item{padding: 30rpx 4%; align-items: flex-start;}
	.person-phtoto{width: 60rpx;height: 60rpx;border-radius: 50%;background: #e1e1e1;overflow: hidden;}
	.person-phtoto image{width: 100%;height: 100%;display: block;border-radius: 50%;}
	
	.seltIcon{width: 36rpx;height: 36rpx; display: block;margin-right: 10rpx;}
	
	.upImg{width: 350rpx; height: 220rpx;background: #F5F5F5;}
	.upImg image{width: 100%;height: 100%;}
	
	.upImg .jiaBtn{width:50rpx;height: 50rpx;display: block;}
	
</style>

