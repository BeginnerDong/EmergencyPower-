<template>
	<view>
		<view class="myRowBetween">
				<view class="item flexRowBetween">
					<view class="ll">银行开户名</view>
					<view class="rr">
						<input type="text" placeholder="请输入银行卡卡户名" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">选择银行：</view>
					<view class="rr">
						<picker @change="bindPickerChange" :value="index" :range="array">
							<view class="uni-input">{{array[index]}}</view>
						</picker>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				
				<view class="item flexRowBetween">
					<view class="ll">银行卡卡号：</view>
					<view class="rr">
						<input type="number" placeholder="请输入银行卡卡号" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">手机号码：</view>
					<view class="rr">
						<input type="number" placeholder="请输入联系电话" maxlength="11" onkeyup="this.value=this.value.replace(/\D/g,'')" placeholder-class="placeholder">
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">验证码：</view>
					<view class="rr">
						<view class="yam" style="width: 50%;">
							<input type="number" style="text-align: center;" placeholder="请输入验证码" placeholder-class="placeholder">
						</view>
						<view class="yzmBtn pubColor mgl20" >获取验证码</view>
					</view>
				</view>
				
				<view class="submitbtn" style="margin-top: 160rpx;">
					<button class="btn" type="submit">保存</button>
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
				score:'',
				wx_info:{},
				is_show:false,
				index: 0,
				curr:1,
				array:['中国银行','工商银行','招商银行','广大银行','浦发银行']
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(curr){
				const self=this
				if(curr!=self.curr){
					self.curr=curr
				}
			},
			bindPickerChange(e) {
				// 搜索选择分类
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';

				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');

				};
				self.$apis.orderGet(postData, callback);

			},

		},
	};
</script>

<style>
	@import "../../assets/style/user.css";
	@import "../../assets/style/editInfor.css";
	
	.myRowBetween .item{padding: 30rpx 4%;border-bottom: 1px solid #eee;}
	

</style> 
