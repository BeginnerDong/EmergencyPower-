<template>
	<view class="">
		
		<view class="proRowList whiteBj">
			<view class="item pdtb15 mglr4 flexRowBetween">
				<view class="ll">
					<image src="../../static/images/details-img2.png"></image>
				</view>
				<view class="rr">
					<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
					<view class="Bmny flexRowBetween">
						<view class="price fs14">56.00</view>
						<view class="numBox flex">
							<view @click="counter('-')">-</view>
							<view class="num pubColor">{{count}}</view>
							<view @click="counter('+')">+</view>
						</view>
					</view>
					
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="myRowBetween fs13 whiteBj">
			<view class="item flexRowBetween">
				<view class="ll">首付给服务商金额</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">联系人</view>
				<view class="rr">
					<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">联系电话</view>
				<view class="rr">
					<input type="text" maxlength="11" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">日期</view>
				<view class="rr">
					<view class="fs13">2019.12.10 11:35</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll" style="width: 20%;">地址</view>
				<view class="rr" style="width: 80%;">
					<view class="fs13">陕西省西安市长安区韦曲街道</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="underFix flexEnd">
			<view class="mgr25">
				合计：<text class="price">560</text>
			</view>
			
			<view class="btn" @click="Utils.stopMultiClick(submit)">立即购买</view>
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
				spaceDate:[{},{},{},{},{},{}],
				count:1
			}
		},
		onLoad(options) {
			const self = this;
		},
		methods: {
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			},
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/proList.css";
	@import "../../assets/style/editInfor.css";
	/* @import "../../assets/style/confirmOrder.css"; */
	
	page{padding-bottom: 120rpx;background: #F5F5F5;}
	.myRowBetween .item{padding: 30rpx 4%;}
	.proRowList .item:last-child{border-bottom: 0;}
	
</style>
