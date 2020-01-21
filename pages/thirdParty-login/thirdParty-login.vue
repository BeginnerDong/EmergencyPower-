<template>
	<view v-if="showAll">
		<view><image style="width: 100%;height: 390rpx;" src="../../static/images/the-login-icon2.png" mode="widthFix"></image></view>
		
		<view>
			<view class="loginEdit">
				<view class="item flex">
					<view class="ll">
						<image class="icon" src="../../static/images/the-login-icon1.png" mode=""></image>
					</view>
					<view class="rr">
						<input type="text" v-model="submitData.login_name" placeholder="输入手机号码" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">
						<image class="icon" src="../../static/images/the-login-icon.png" mode=""></image>
					</view>
					<view class="rr flexRowBetween">
						<input type="password" v-model="submitData.password" placeholder="输入密码" placeholder-class="placeholder" />
					</view>
				</view>
				<!-- <view class="item flexEnd"><view class="fs13 color6" @click="Router.navigateTo({route:{path:'/pages/thirdParty-revisePswd/thirdParty-revisePswd'}})">修改密码</view></view> -->
			</view>
			
			<view class="submitbtn" style="margin-top: 120rpx;">
				<button class="btn" type="button" @click="submit">登录</button>
				<view class="fs13 color6 pdtb10" style="width: 80%;margin: 0 auto;" @click="Router.navigateTo({route:{path:'/pages/thirdParty-register/thirdParty-register'}})">没有账号去注册？</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad(options) {
			const self = this;
			if (uni.getStorageSync('thirdToken')&&uni.getStorageSync('thirdInfo').user_type==1) {
				uni.redirectTo({
					url: '/pages/thirdPartyUser/thirdPartyUser'
				})
			}else{
				self.showAll = true
			}
		},
		
		methods: {
			
			submit() {
				const self = this;
			
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					
					const callback = (res) => {
						if (res.solely_code == 100000) {
							uni.login({
								success(data) {
									const c_postData = {
										code: data.code,
										token:res.token
									};
									const c_callback = (c_res) => {
										if (c_res.solely_code == 100000) {
											uni.setStorageSync('thirdToken', res.token);
											uni.setStorageSync('thirdInfo', res.info);
											uni.redirectTo({
												url: '/pages/thirdPartyUser/thirdPartyUser'
											}) 
										} else {
											self.$Utils.showToast(c_res.msg,'none')
										}
									}
									self.$apis.bindWechat(c_postData, c_callback);
								}
							})
							console.log(res);
							
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.shopLogin(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
			
		},
	};
</script>

<style>
.loginEdit{padding: 0 8%;}
.loginEdit .item{padding-top: 40rpx;}
.loginEdit .item .ll{width: 10%;}
.loginEdit .item .icon{width: 44rpx; height: 44rpx;}
.loginEdit .item .rr{width: 90%; padding: 10rpx 0;border-bottom: 1px solid #eee;font-size: 26rpx;}
.loginEdit .item .rr input{width: 100%;line-height: 60rpx;height: 60rpx; display: block;font-size: 24rpx;}
.loginEdit .item .yzm{color: #f6af15;}
</style>

