<template>
	<view>
		<view class="orderNav pdlr4 whiteBj flexRowBetween color6">
			<!-- <view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">应急发电</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">工程器械</view>
			<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">操作人员</view> -->
			<view class="tt" :class="labelId==item.id?'on':''" @click="changeCurr(index+1,item.id)" v-for="(item,index) in labelData">
				{{item.title}}
			</view>
		</view>
		<view class="pdtb20 mgb15"></view>
		<view class="pdlr4">
			<view class="proRowList" v-show="curr==1">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view class="flexRowBetween pdb10 fs12">
						<view class="color9">{{item.create_time}}</view>
						<view class="color6">状态：
							<text class="state pubColor" v-if="item.check_status==1">已发布</text>
							<text class="state pubColor" v-if="item.check_status==0">审核中</text>
							<text class="state pubColor" v-if="item.check_status==-1">已拒绝</text>
						</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">{{item.title}}</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">{{item.price}}</view>
								<view class="color6 fs12">库存：{{item.stock}}</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdb5 pdt15">
						<view class="flex fs12 color6">
							<view class="flex mgr25" :data-id="item.id" @click="productUpdate($event.currentTarget.dataset.id)">
								<image class="editIcon" src="../../static/images/address-icon3.png" mode="">删除</image>
								</image>
							</view>
							<view class="flex" :data-id="item.id"
							@click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGlAdd/thirdParty_serviceGlAdd?id='+$event.currentTarget.dataset.id+'&label_id='+labelData[curr-1].id}})">
								<image class="editIcon" src="../../static/images/address-icon2.png" mode="">编辑</image>
								</image>
							</view>
						</view>
					</view>

				</view>
			</view>
			<view class="proRowList" v-show="curr==2">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view class="flexRowBetween pdb10 fs12">
						<view class="color9">{{item.create_time}}</view>
						<view class="color6">状态：
							<text class="state pubColor" v-if="item.check_status==1">已发布</text>
							<text class="state pubColor" v-if="item.check_status==0">审核中</text>
							<text class="state pubColor" v-if="item.check_status==-1">已拒绝</text>
						</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">{{item.title}}</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">{{item.price}}</view>
								<view class="color6 fs12">库存：{{item.stock}}</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdb5 pdt15">
						<view class="flex fs12 color6">
							<view class="flex mgr25" :data-id="item.id" @click="productUpdate($event.currentTarget.dataset.id)">
								<image class="editIcon" src="../../static/images/address-icon3.png" mode="">删除</image>
								</image>
							</view>
							<view class="flex" :data-id="item.id"
							@click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGlAdd/thirdParty_serviceGlAdd?id='+$event.currentTarget.dataset.id+'&label_id='+labelData[curr-1].id}})">
								<image class="editIcon" src="../../static/images/address-icon2.png" mode="">编辑</image>
								</image>
							</view>
						</view>
						<div class="underBtn flexEnd">
							<span class="Bbtn" :data-no="item.product_no"
							 @click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGl_specsAdd/thirdParty_serviceGl_specsAdd?product_no='+$event.currentTarget.dataset.no}})">添加规格</span>
						</div>
					</view>

				</view>
			</view>
			<view class="proRowList" v-show="curr==3">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view class="flexRowBetween pdb10 fs12">
						<view class="color9">{{item.create_time}}</view>
						<view class="color6">状态：
							<text class="state pubColor" v-if="item.check_status==1">已发布</text>
							<text class="state pubColor" v-if="item.check_status==0">审核中</text>
							<text class="state pubColor" v-if="item.check_status==-1">已拒绝</text>
						</view>
					</view>
					<view class="flexRowBetween">
						<view class="ll">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">{{item.title}}</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs14">{{item.price}}</view>
								<view class="color6 fs12">库存：{{item.stock}}</view>
							</view>
						</view>
					</view>
					<view class="flexEnd pdb5 pdt15">
						<view class="flex fs12 color6">
							<view class="flex mgr25" :data-id="item.id" @click="productUpdate($event.currentTarget.dataset.id)">
								<image class="editIcon" src="../../static/images/address-icon3.png" mode="">删除</image>
								</image>
							</view>
							<view class="flex" :data-id="item.id" 
							@click="Router.navigateTo({route:{path:'/pages/thirdParty_serviceGlAdd/thirdParty_serviceGlAdd?id='+$event.currentTarget.dataset.id+'&label_id='+labelData[curr-1].id}})">
								<image class="editIcon" src="../../static/images/address-icon2.png" mode="">编辑</image>
								</image>
							</view>
						</view>
					</view>

				</view>
			</view>


		</view>

		<view class="fabubtn" @click="submit()">
			<image class="icon" src="../../static/images/service-icon.png" mode=""></image>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				showView: false,
				wx_info: {},
				is_show: false,
				curr: 1,
				seviceDate: [{}, {}],
				labelData: [],
				mainData: [],
				labelId: -1
			}
		},

		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			
		},
		
		onShow() {
			const self = this;
			self.mainData = [];
			self.$Utils.loadAll(['getLabelData'], self);
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
			
			productUpdate(id) {
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
							self.$apis.productUpdate(postData, callback)
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			
			submit(){
				const self = this;
				//self.Router.navigateTo({route:{path:'/pages/thirdParty_serviceGlAdd/thirdParty_serviceGlAdd?label_id='+self.labelData[self.curr-1].id}})
				//return
				if(self.labelData[self.curr-1].order.length>0){
					self.Router.navigateTo({route:{path:'/pages/thirdParty_serviceGlAdd/thirdParty_serviceGlAdd?label_id='+self.labelData[self.curr-1].id}})
				}else{
					self.$Utils.showToast('您还没有购买此服务类别','none');
				}
			},
			
			changeCurr(curr, id) {
				const self = this;
				if (curr != self.curr) {
					self.curr = curr;
					self.labelId = id
					self.getChildLabel()
				}
			},

			getLabelData() {
				const self = this;
				self.labelData = [];
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type: 3,
				};
				postData.order = {
					listorder: 'desc'
				}
				postData.getAfter = {
					order: {
						token: uni.getStorageSync('thirdToken'),
						tableName: 'Order',
						middleKey: 'id',
						key: 'category_id',
						searchItem: {
							status: 1,
							pay_status: 1,
							type: 2
						},
						condition: '='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
						self.labelId = self.labelData[0].id
					}
					self.getChildLabel()
					console.log('self.labelData', self.labelData)
				};
				self.$apis.labelGet(postData, callback);
			},

			getChildLabel() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					id: self.labelId
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.idArray = res.info.data
						self.getMainData(true)
					}
					console.log('self.labelData', self.labelData)
				};
				self.$apis.childLabelGet(postData, callback);
			},

			getMainData(isNew) {
				const self = this;
				
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
				postData.tokenFuncName = 'getThirdToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					category_id: ['in', self.idArray],
					type: 1,
					user_no:uni.getStorageSync('thirdInfo').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";

	page {
		padding-bottom: 110rpx;
		background: #F5F5F5;
	}

	.orderNav {
		position: fixed;
		top: 0rpx;
		left: 0;
		right: 0;
		z-index: 22;
	}

	.orderNav .tt {
		width: 33.3%;
	}

	.proRowList .item {
		padding: 20rpx;
		margin-bottom: 30rpx;
		border-bottom: 0;
		border-radius: 10rpx;
	}

	.proRowList .item .ll {
		width: 180rpx;
		height: 180rpx;
	}

	.proRowList .rr {
		width: 70%;
		height: 160rpx;
	}

	.give-input {
		width: 200rpx;
	}

	.give-input input {
		width: 100%;
		text-align: right;
		font-size: 26rpx;
	}

	.underBtn .Bbtn {
		display: block;
		width: 120rpx;
		text-align: center;
		line-height: 50rpx;
		text-align: center;
		margin-left: 30rpx;
		border-radius: 8rpx;
		font-size: 24rpx;
		background: #0094DC;
		color: #fff;
	}

	.editIcon {
		width: 30rpx;
		height: 30rpx;
		margin-right: 10rpx;
	}

	/* 页面右侧固定按钮 */
	.fabubtn {
		width: 120rpx;
		font-size: 28rpx;
		text-align: center;
		position: fixed;
		bottom: 20%;
		right: 20rpx;
		z-index: 66;
	}

	.fabubtn .icon {
		width: 90rpx;
		height: 90rpx;
		border-radius: 50%;
		display: block;
		margin: 8rpx auto;
		box-shadow: 0 0 10rpx rgba(0, 0, 0, 0.12);
	}
</style>
