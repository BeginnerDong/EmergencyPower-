<template>
	<view>
		<view class="whiteBj mglr4 pdb15 refOrder">
			<view class="mgt15">
				<view class="proRowList">
					<view class="item">
						<view class="flexRowBetween pdb10">
							<view class="fs12 color9">交易时间：2000.0.1</view>
							<view class="fs12 red">已退款</view>
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
				
			<view class="refCon">
				你速度hi速度吃uhviuosfhniudfhiuadfasddc大多数深V水电费v水电费v是是地方吃素的 
			</view>
		
		</view>
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
	
	.proRowList .item{padding: 20rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 160rpx; height: 160rpx;}
	.proRowList .rr{width: 72%;height: 160rpx;}
	
	.refOrder{border-radius: 10rpx;}
	.refCon{margin: 0 20rpx;background-color: #f5f5f5;border-radius: 10rpx;padding: 30rpx;font-size: 24rpx;color: #222;}
</style>
