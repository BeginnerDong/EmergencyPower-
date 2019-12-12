<template>
	<view class="" >

		<view class="pdlr4 pdt15 pdb15 fs13 whiteBj">
			<view class="pdb10">服务地址</view>
			<view class="flexRowBetween"@click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
				<view class="" style="width: 90%;">
					<view class="flexRowBetween mgb5" style="align-items: flex-start;">
						<view style="width: 70%;">
							<view>门店名称门店名称<text class="mgl15">刘丹</text></view>
						</view>
						<view class="fs11 color9">设备总功率:53kw</view>
					</view>
					<view class="color6">陕西省西安市雁塔区高新大都荟</view>
				</view>
				<view class="flexEnd"><image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="proRowList whiteBj">
			<view class="item pdtb15 mglr4 flexRowBetween">
				<view class="ll">
					<image src="../../static/images/details-img2.png"></image>
				</view>
				<view class="rr">
					<view class="avoidOverflow2 fs13">标题标题标题标题标题标题标题标题标题标题标题标题标题标题</view>
					<view class="Bmny">
						<view class="price fs14">56.00/小时</view>
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
				<view class="ll">联系电话</view>
				<view class="rr">
					<input type="text" maxlength="11" value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">预约时间</view>
				<view class="rr">
					<view class="fs13">2019.12.10 11:35</view>
					<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" style="align-items: flex-start;">
				<view style="20%">时长</view>
				<view style="width: 80%;">
					<view class="flexEnd seltDay center fs13">
						<view class="btn">4小时</view>
						<view class="btn">8小时</view>
						<view class="btn on">自定义</view>
					</view>
					<view class="flexEnd">
						<view class="borderB1 pdb5 pdt5" style="width: 320rpx;">
							<input type="number" value="" placeholder="输入时长"  placeholder-class="placeholder"/>
						</view>
					</view>
				</view>
			</view>
			<view class="item flexEnd">
				<view>超过5公里，1公里收取20元</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show" style="bottom: 120rpx;"></view>
		
		<!-- 合计弹框 -->
		<view class="qjAlertCont costShow borderB1" v-show="is_costShow">
			<view class="mglr4 pdtb15 center fs15 borderB1">费用明细</view>
			<view class="infor fs13 pdb20">
				<view class="item flexRowBetween" v-for="(item,index) in costDate" @click="seltCost(index)">
					<span>{{item.title}}</span>
					<view>{{item.text}}</view>
				</view>
			</view>
		</view>
		
		<view class="underFix flexRowBetween">
			<view class="flex"  @click="costShow">
				合计：<view class="price">560</view>
				<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
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
				costDate:[
					{title:'单价',text:'￥56'},
					{title:'时长',text:'9小时'},
					{title:'距离',text:'6公里'},
					{title:'距离费用',text:'￥20'},
					{title:'距离费用',text:'￥20'},
					{title:'距离费用',text:'￥20'}
				],
				seltCur:false,
				is_costShow:false
			}
		},
		onLoad(options) {
			const self = this;
		},
		methods: {
			costShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_costShow = !self.is_costShow
			},
			seltCost(index){
				const self = this;
				self.seltCur = index
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
	@import "../../assets/style/confirmOrder.css";
	
	page{padding-bottom: 120rpx;background: #F5F5F5;}
	
	.proRowList .item:last-child{border-bottom: 0;}
	
</style>
