<template>
	<view>
		
		<view class="pdlr4 mgt15">
			<view class="proRowList">
				<view class="item">
					<view class="flexRowBetween pdb10">
						<view class="fs12 color9">交易时间：2000.0.1</view>
						<view class="fs12 red">待确认</view>
						<!-- <view class="fs12 red" v-if="item.accept==1&&item.transport_status!=2">进行中</view>
						<view class="fs12 red" v-if="item.transport_status==2&&item.isremark==0">已完成</view>
						<view class="fs12 red" v-if="item.isremark==1">已评价</view> -->
					</view>
					<view class="flexRowBetween pdb15">
						<view class="ll">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
							item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">标题标题</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">685</view>
								<view class="fs10 color6">x1</view>
							</view>
						</view>
					</view>
					<view class="flexEnd fs13">
						<view>共1件商品 合计：￥56</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="color2 pdt5 pdb15 pdlr4">退款/售后原因</view>
		<view class="pdlr4 pdt-15">
			<textarea value="" placeholder="请写出您的退款的原因,帮助您更快的审核通过~" class="refundCon"/>
		</view>
		
		<view class="btn">提交</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 160rpx; height: 160rpx;}
	.proRowList .rr{width: 72%;height: 160rpx;}
	
	.refundCon{width: 690rpx;height: 300rpx;background-color: #fff;border-radius: 10rpx;}
	.btn{ height: 80rpx; text-align: center; line-height: 80rpx; color:#fff; background: #0094DC; border-radius: 10rpx;margin: 12% 10% 0;}
	.uni-input-placeholder{color: #999;}
	textarea{font-size: 24rpx;color: #999;padding: 30rpx;}
</style>
