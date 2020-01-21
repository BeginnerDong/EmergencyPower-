<template>
	<view>
		<view class="detailxqBan">
			<swiper class="detailxqBan"  indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#0094dc">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image"/>
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="detailTit mglr4 pdtb10">
			<view class="tit">{{mainData.title}}</view>
			<view class="fs13 color6 pdt5 pdb5">{{mainData.description}}</view>
			<view class="flexRowBetween">
				<view class="price">{{mainData.price}}</view>
				<view class="fs10 color9">销量：{{mainData.sale_count}}</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="pdlr4">
			<view class="fs15 pdt15 pdb10 ftw">服务介绍</view>
			<view class="xqInfor">
				<view class="cont fs13 color6">
					<view class="content ql-editor" style="padding:0;" 
					v-html="mainData.content">
					</view>
					<view v-for="item in mainData.contentImg">
						<image :src="item.url" />
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="pingjia mglr4">
			<view class="flexRowBetween pdt15 pdb5">
				<view class="flex ftw fs15">评价</view>
				<view class="color9 fs12 flexEnd" 
				@click="Router.navigateTo({route:{path:'/pages/pingjiaList/pingjiaList?id='+id}})">更多<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image></view>
			</view>
			<view class="pjList pdtb15" v-for="(item,index) in messageData" :key="index"  
			v-if="messageData.length>0">
				<view class="flexRowBetween pdb10">
					
					<view class="flex">
						<image class="us-photo mgr10" 
						:src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						<view class="color6">
							<view class="fs13">{{item.title}}</view>
							<view class="fs10">{{item.create_time}}</view>
						</view>
					</view>
					<view class="xing flexEnd">
					
						<view class="starBox mgr5">
							<image v-for="c_item in stars" :src="item.score > c_item ?(item.score/2-c_item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
							
							</image>
						</view>
						<view class="fs12 color9">{{item.score}}分</view>
					</view>
				</view>
				
				<view class="fs13">{{item.description}}</view>
			</view>
			<view  style="text-align: center;height: 100rpx;line-height: 100rpx;">
				暂无评价~
			</view>
		</view>
		
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar">
			<view class="left">
				<view class="ite" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<image src="../../static/images/details-icon3.png" mode=""></image>
					<view>返回首页</view>
				</view>
				<view class="ite" @click="collect()">
					<image :src="mainData.collectMe.length>0&&mainData.collectMe[0].status==1?
					'../../static/images/details-icon5-a.png':'../../static/images/details-icon5.png'" mode=""></image>
					<view>收藏</view>
				</view>
				<button class="ite" open-type="contact">
					<image src="../../static/images/details-icon4.png" mode=""></image>
					<view>客服</view>
				</button>
			</view>
			<view class="payBtn" @click="Utils.stopMultiClick(goBuy)">立即下单</view>
		</view>
	</view>

</template>


<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				pingjiaList:[{},{},{}],
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 5
				},
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/details-icon.png',
				selectedSrc: '../../static/images/details-icon1.png',
				halfSrc: '../../static/images/details-icon1-a.png',
				mainData:{},
				messageData:[],
				orderList:[]
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro')
		},
		

		methods: {
			
			goBuy(){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.orderList.push(
					{product_id:self.mainData.id,count:1,
					type:1,product:self.mainData},
				);
				uni.setStorageSync('payPro', self.orderList);
				if(self.mainData.origin_id==1){
					self.Router.navigateTo({route:{path:'/pages/emergency-confirmOrder/emergency-confirmOrder'}})
				}else if(self.mainData.origin_id==2){
					self.Router.navigateTo({route:{path:'/pages/confirmOrder/confirmOrder'}})
				}else if(self.mainData.origin_id==3){
					self.Router.navigateTo({route:{path:'/pages/operator-confirmOrder/operator-confirmOrder'}})
				}
				uni.setStorageSync('canClick',true);
			},
			
			collect(){
				const self = this;
				if(self.mainData.collectMe.length>0){
					self.logUpdate()
				}else{
					self.logAdd()
				}
			},
			
			logUpdate() {
				const self = this;
				
				const postData = {
					tokenFuncName: 'getProjectToken',
					searchItem:{
						id:self.mainData.collectMe[0].id
					}
				};
				postData.data = {
				};
				if(self.mainData.collectMe[0].status==1){
					postData.data.status=-1
				}else{
					postData.data.status=1
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						if(self.mainData.collectMe[0].status==1){
							self.$Utils.showToast('取消成功', 'none');
						}else{
							self.$Utils.showToast('收藏成功', 'none');
						}
						setTimeout(function() {
							self.getMainData()
						}, 1000);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.logUpdate(postData, callback);
			},
			
			logAdd() {
				const self = this;
				
				const postData = {
					tokenFuncName: 'getProjectToken',
				};
				postData.data = {
					thirdapp_id:2,
					relation_id:self.mainData.id,
					relation_table:'product',
					type:2,
					user_no:uni.getStorageSync('user_info').user_no,
					behavior:self.mainData.category_id
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.$Utils.showToast('收藏成功', 'none');
						setTimeout(function() {
							self.getMainData()
						}, 1000);
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.logAdd(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				postData.getAfter = {

					collectMe:{
						
						tableName:'Log',
						middleKey:'id',
						key:'relation_id',
						searchItem:{
							status:1,
							type:2,
							relation_table:'product',
							user_no:uni.getStorageSync('user_info').user_no,
							status:['in',[1,-1]],
							
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.getMessageData();
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getMessageData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					relation_id :self.id,
					type:1,
					user_type:0
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData.push.apply(self.messageData, res.info.data);
					}
					console.log('23',res.info.total)
					self.totalMessage = res.info.total;
					console.log('self.messageData', self.messageData)
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/detial.css";
	@import "../../assets/style/quill.css";
	@import "../../assets/style/star.css";
	button{
		background: none;
		line-height: 1.5;
		margin: 0;
	}
	button::after{
		border: none;
	}
	page{padding-bottom: 120rpx;}
	
</style>
