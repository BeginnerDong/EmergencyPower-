<template>
	<view>
		
		<view class="pdlr4">
			<view class="rankingNav flexRowBetween fs13 color6 pdtb15">
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
							<view class="price">{{item.price}}/天</view>
							<view class="fs10 color9">销量:{{item.sale_count}}</view>
						</view>
						
					</view>
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
				produtList: [{},{},{},{},{},{}],
				order:{},
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
			self.$Utils.loadAll(['getLocation'], self);
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
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['工程机械']],
						},
						middleKey: 'category_id',
						key: 'id',
						condition: 'in',
					},
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					check_status:1,
					type:1
				};
				if (JSON.stringify(self.order) != '{}') {
					postData.order = self.$Utils.cloneForm(self.order);
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getLocation');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/proList.css";
</style>
