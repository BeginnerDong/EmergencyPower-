<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flexRowBetween">
					<view class="ll">公司名称</view>
					<view class="rr">
						<input type="text" v-model="submitData.name" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">所在地</view>
					<div class="rr " @click="chooseAddress()">
						<input type="color9" placeholder="选择您的位置" disabled="true"   v-model="submitData.address">
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</div>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">常用联系人</view>
					<view class="rr">
						<input type="text" v-model="submitData.contacts" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">联系人电话</view>
					<view class="rr">
						<input type="text" v-model="submitData.phone" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">联系人邮箱</view>
					<view class="rr">
						<input type="text" v-model="submitData.email" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="mgr15" style="width: 25%;">营业执照上传</view>
					<view class="flex" style="width:75%;flex-wrap: wrap;">
						
						<view class="flexCenter upImg radius8" v-for="item in submitData.license_img" v-if="submitData.license_img.length>0">
						<image :src="item.url" mode="">
							
						</image></view>
						<view class="flexCenter upImg radius8" @click="upLoadImg('license_img')">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image>
						</view>
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
				showView: false,
				wx_info:{},
				is_show:false,
				currSex:0,
				submitData:{
					name:'',
					address:'',
					contacts:'',
					phone:'',
					email:'',
					license_img:[],
					identity:1
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			chooseAddress(e) {
				const self = this;
				uni.authorize({
				    scope: 'scope.userLocation',
				    success() {
				        uni.chooseLocation({
				        	success: (res) => {
				        		console.log(res)
				        		self.submitData.address = res.address;
				        	},
				        	fail: (e) => {
				        		uni.getSetting({
				        			success: (res) => {
				        				console.log(res)
				        				let locaAuth = res.authSetting['scope.userLocation']
				        				if (locaAuth) {/* 判断位置是否已经授权，是选择地图位置点击取消触发的fail，再选择位置 */
				        					console.log('地图点击取消')
				        					uni.chooseLocation({
				        						success: (res) => {
				        							self.submitData.address = res.address;
				        							
				        						},
				        					});
				        				}
				        				if (!locaAuth) { /* 如果地理位置没授权 */
				        					console.log(222)
				        					uni.showModal({
				        					    title: '提示',
				        					    content: '需要授权位置信息',
				        						confirmColor:'#ca1c1d',
				        						showCancel:true,
				        					    success: function (res) {
				        					        if (res.confirm) {
				        					            uni.openSetting({
				        					            	success: (res) => {
				        					            		console.log(res.authSetting)
				        					            	},
				        					            	fail: (res) => {
				        					            		console.log(res)
				        					            	},
				        					            });
				        					        } else if (res.cancel) {
				        					           
				        					        }
				        					    }
				        					});			
				        					
				        				
				        				}
				        			}
				        		})
				        	}
				        });
				    },
					fail: (e) => {
						uni.showModal({
						    title: '提示',
						    content: '需要授权位置信息',
							confirmColor:'#ca1c1d',
							showCancel:true,
						    success: function (res) {
						        if (res.confirm) {
						            uni.openSetting({
						            	success: (res) => {
						            		console.log(res.authSetting)
						            	},
						            	fail: (res) => {
						            		console.log(res)
						            	},
						            });
						        } else if (res.cancel) {
						           
						        }
						    }
						});
					}
				})
				
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
							data:1
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
						self.submitData.license_img = self.mainData.license_img
						self.submitData.name = self.mainData.name
						self.submitData.phone = self.mainData.phone
						self.submitData.email = self.mainData.email
						self.submitData.address = self.mainData.address
						self.submitData.contacts = self.mainData.contacts
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
	
	.upImg{width: 200rpx; height: 230rpx;background: #F5F5F5;margin-right: 20rpx;}
	.upImg image{width: 100%;height: 100%;}
	.upImg .jiaBtn{width:50rpx;height: 50rpx;display: block;}
	
</style>

