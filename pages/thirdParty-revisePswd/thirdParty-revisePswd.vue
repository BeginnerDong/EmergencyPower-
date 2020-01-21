<template>
	<view>
		<view><image style="width: 100%;height: 390rpx;" src="../../static/images/the-login-icon2.png" mode="widthFix"></image></view>
		
		<view>
			<view class="loginEdit">
				<view class="item flex">
					<view class="ll">
						<image class="icon" src="../../static/images/the-login-icon.png" mode=""></image>
					</view>
					<view class="rr">
						<input type="text" value="" placeholder="输入初始密码" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">
					</view>
					<view class="rr flexRowBetween">
						<input type="text" value="" placeholder="输入新密码" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">
					</view>
					<view class="rr flexRowBetween">
						<input type="text" value="" placeholder="确认输入新密码" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 120rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/thirdParty-login/thirdParty-login'}})">确定</button>
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
				index: 0,
				is_show:false,
				type:'',
				mode: '',
				submitData:{
					phone:'',
					password:'',
					passwordCopy:''
				}
			}
		},
		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
		},
		
		methods: {
			
			
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {
					if(self.submitData.password!=self.submitData.passwordCopy){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('两次输入密码不一致', 'none');
						return
					};
					self.passwordUpdate();					
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			passwordUpdate() {
				const self = this;
				const postData = {};
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.passwordCopy;
				delete newObject.code;
				postData.data = {};
				postData.data = self.$Utils.cloneForm(newObject);
				postData.smsAuth = {
					phone:self.submitData.phone,						
					code:self.submitData.code
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('修改成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.resetPassword(postData, callback);
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

