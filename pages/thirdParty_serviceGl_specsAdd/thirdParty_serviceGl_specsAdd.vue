<template>
	<view>
		<view class="pdlr4">
			<view class="specslist">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view>{{item.title}}</view>
					<view class="flexRowBetween pdtb10">
						<view class="price">{{item.price}}</view>
						<view class="fs12 color6">库存：{{item.stock}}</view>
					</view>
					<view class="flexEnd">
						<view class="flex fs12 color6">
							<view class="flex mgr25" :data-id="item.id" @click="skuUpdate($event.currentTarget.dataset.id)">
								<image class="editIcon" src="../../static/images/address-icon3.png" mode="">
									删除
									</image>
									</image>
								</view>
							<view class="flex" :data-id="item.id" 
							@click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGl_specsEdit/thirdParty_serviceGl_specsEdit?id='+$event.currentTarget.dataset.id+'&product_no='+product_no}})"><image class="editIcon" 
							src="../../static/images/address-icon2.png" mode="">编辑</image></image>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="fabubtn" @click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGl_specsEdit/thirdParty_serviceGl_specsEdit?product_no='+product_no}})">
			<image class="icon" src="../../static/images/service-icon.png" mode=""></image>
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
				specsDate:[{},{},{}],
				mainData:[],
				product_no:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.product_no = options.product_no;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			skuUpdate(id) {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认是否删除此服务',
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.searchItem = {};
							postData.searchItem.id = id;
							postData.tokenFuncName = 'getThirdToken';
							postData.data = {
								status:-1
							};
							const callback = (res) => {
								if (res.solely_code==100000) {
									self.getMainData(true);
								}else{
									self.$Utils.showToast(res.msg,'none');	
								}
							};
							self.$apis.skuUpdate(postData, callback)
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			
			getMainData(isNew) {
				const self = this;
				self.mainData = [];
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					product_no:self.product_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.skuGet(postData, callback);
			},
		}
	};
</script>

<style>
	
	page{padding-bottom: 60rpx;}
	.specslist .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.editIcon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	
	/* 页面右侧固定按钮 */
	.fabubtn{ width: 120rpx;font-size: 28rpx; text-align: center; position: fixed; bottom: 20%;right: 20rpx; z-index: 66;}
	.fabubtn .icon{width: 90rpx; height: 90rpx;border-radius: 50%;display: block;margin:8rpx auto;box-shadow: 0 0 10rpx rgba(0,0,0,0.12);}
	
</style>

