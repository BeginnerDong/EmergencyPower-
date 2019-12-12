<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flex">
					<view class="ll">手机号</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">验证码</view>
					<view class="rr">
						<view style="width: 50%;"><input type="text" value="" placeholder="输入验证码" placeholder-class="placeholder" style="text-align: center;" /></view>
						<view class="mgl20 pubColor">获取验证码</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">密码</view>
					<view class="rr">
						<input type="text" value="" placeholder="确认输入密码" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">选择入驻类目</view>
					<view class="rr">
						<picker @change="leimuChange" :value="index" :range="leimuDate">
							<view class="color9">{{leimuDate[index]}}</view>
						</picker>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">选择入驻原因</view>
					<view class="rr">
						<picker @change="reasonChange" :value="reasonNum" :range="reasonDate">
							<view class="color9">{{reasonDate[reasonNum]}}</view>
						</picker>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">营业执照</view>
					<view class="rr">
						<view class="flexCenter upImg"><image src="../../static/images/details-img2.png" mode=""></image></view>
						<view class="flexCenter upImg"><image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">特种经营许可证</view>
					<view class="rr">
						<view class="flexCenter upImg"><image src="../../static/images/details-img2.png" mode=""></image></view>
						<view class="flexCenter upImg"><image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">其他资质</view>
					<view class="rr">
						<view class="flexCenter upImg"><image src="../../static/images/details-img2.png" mode=""></image></view>
						<view class="flexCenter upImg"><image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image></view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">往期经营报告</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 80rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/thirdParty-login/thirdParty-login'}})">确定</button>
				<view class="agreeSel flex pdt10">
					<view class="flex">
						<view class="selt" @click="xieyiSelt">
							<image v-show="!seltCurr" src="../../static/images/address-icon1.png" mode=""></image>
							<image v-show="seltCurr" src="../../static/images/address-icon.png" mode=""></image>
						</view>
						<view class="fs13" @click="xieyiShow">《入驻协议》</view>
					</view>
				</view>
			</view>
			
			<view class="xieyiAlert" v-show="is_xieyiShow">
				<view class="infor radius10">
					<view class="cont fs12">
						<view class="fs14 pdb20 center">入驻协议</view>
						<view>1、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
						<view>2、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
						<view>3、内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
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
				leimuDate:['请选择','应急发电','工程机械','操作人员'],
				index:0,
				reasonNum:0,
				reasonDate:['请选择','我要雇人办事','我要入驻赚钱'],
				is_xieyiShow:false,
				seltCurr:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			leimuChange(e) {
				// 搜索选择分类
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value
			},
			reasonChange(e) {
				// 搜索选择分类
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.reasonNum = e.target.value
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

