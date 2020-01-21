<template>
	<view>
		
		<view class="banner-box pdlr4">
			<view class="banner pdtb15">
				<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#0094dc">
					<block v-for="(item,index) in sliderData.mainImg" :key="index">
						<swiper-item class="swiper-item">
							<image :src="item.url" class="slide-image"/>
						</swiper-item>
					</block>
				</swiper>
			</view>
		</view>
		
		<view class="indHome pdlr4 flex fs14">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/emergency/emergency'}})">
				<image src="../../static/images/home-icon.png"></image>
				<view class="tit">应急发电</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/mechanics/mechanics'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">工程机械</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/operator/operator'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">操作人员</view>
			</view>
		</view>
		
		<view class="f5H10"></view>
		
		<view class="pdlr4">
			<view class="fs15 pdtb20 ftw">热门推荐</view>
			<view class="rankingNav flexRowBetween fs13 color6 pdb15">
				<view class="item flexCenter" @click="orderChange('sale_count')">
					<view>销量</view>
					<view class="fs-down flexColumn mgl5">
						<view class="arrow top" 
						:class="orderItem=='sale_count'&&orderType=='asc'?'on':''"></view>
						<view class="arrow bottom" 
						:class="orderItem=='sale_count'&&orderType=='desc'?'on':''"></view>
					</view>
				</view>
				<view class="item flexCenter" @click="orderChange('price')">
					<view>价格</view>
					<view class="fs-down flexColumn mgl5">
						<view class="arrow top"
						:class="orderItem=='price'&&orderType=='asc'?'on':''"></view>
						<view class="arrow bottom"
						:class="orderItem=='price'&&orderType=='desc'?'on':''"></view>
					</view>
				</view>
			</view>
			<view class="proLis flexRowBetween">
				<view class="item-lis boxShaow" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/pageDetail/pageDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
					</view>
					<view class="infor">
						<view class="tit avoidOverflow fs13 pdb5">{{item.title}}</view>
						<view class="flexRowBetween">
							<view class="price">{{item.price}}</view>
							<view class="fs10 color9">销量:{{item.sale_count}}</view>
						</view>
						
					</view>
				</view>
			</view>
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
		
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
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				produtList: [
					{},{},{},{}
				],
				sliderData:{},
				mainData:[],
				order:{},
				orderItem:'distance',
				orderType:'asc'
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLocation','getSliderData'], self);
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
			
			orderChange(item){
				const self = this;
				self.order = {};
				self.orderItem = item;
				if(self.orderType=='desc'){
					self.orderType='asc'
				}else{
					self.orderType='desc'
				};
				self.order = {
					[item]:self.orderType
				};
				
				console.log('self.order',self.order)
				self.getMainData(true)
			},
			
			getLocation() {
				const self = this;
				const callback = (res) => {
					if (res) {
						console.log('res', res)
						if(res.authSetting){
							console.log(232)
							return
						}
						self.lng = res.location.lng;
						self.lat = res.location.lat;
						self.order = {
							distance:'asc',
							longitude:self.lng,
							latitude:self.lat,
						}
						self.getMainData(true)
					};
				};
				self.$Utils.getLocation('reverseGeocoder', callback);
				self.$Utils.finishFunc('getLocation');
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title: '首页轮播图'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data[0]
					}
					console.log('self.sliderData', self.sliderData)
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:1,
					thirdapp_id: 2,
					check_status:1,
					listorder:['>',0]
				};
				if (JSON.stringify(self.order) != '{}') {
					postData.order = self.$Utils.cloneForm(self.order);
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proList.css";
	
	page {padding-bottom: 140rpx;}
	.swiper-box {height: 360rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item {width: 100%;}
	.swiper-box image {width: 100%;height: 100%;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 33.3%;text-align: center;padding-bottom: 15px; font-size: 24rpx; color: #222;display: flex; flex-direction: column;}
	.indHome .item image {width:110rpx;height: 110rpx; border-radius: 50%;margin: 0 auto 20rpx auto; }
	
</style>
