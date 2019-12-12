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
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/staff-login/staff-login'}})">确定</button>
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
				is_show:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
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

