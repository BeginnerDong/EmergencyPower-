<template>
	<view class="" >
		<view class="orderNav pdlr4 whiteBj flexRowBetween">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">静音发电机</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">油机发电机</view>
		</view>
		<view class="pdtb20 mgb15"></view>
		<view class="mglr4" v-show="curr==1">
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
			<view class="proRowList">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/pageDetail/pageDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="ll">
						<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
					</view>
					<view class="rr">
						<view class="avoidOverflow2 fs13">{{item.title}}</view>
						<view class="fs10 color9">{{item.description}}</view>
						<view class="Bmny flexRowBetween">
							<view class="price fs13">{{item.price}}/小时</view>
							<view class="fs10 color6">销量:{{item.sale_count}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view v-show="curr==2">
			<view class="leftNav fs12 color9 center">
				<view class="item" :class="currId==item.id?'on':''" @click="changeSpecsCurr(item.id)" 
				v-for="(item,index) in typeData" :key="index">{{item.title}}</view>
			</view>
			<view class="R-proList">
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
				<view class="proRowList">
					<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/pageDetail/pageDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="ll">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" alt="" />
						</view>
						<view class="rr">
							<view class="avoidOverflow2 fs13">{{item.title}}</view>
							<view class="fs10 color9">{{item.description}}</view>
							<view class="Bmny flexRowBetween">
								<view class="price fs13">{{item.price}}/小时</view>
								<view class="fs10 color6">销量:{{item.sale_count}}</view>
							</view>
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
				curr:1,
				muteProDate:[{},{},{},{}],
				leftNavData:['2KW-4KW','5KW-6KW','9KW-12KW'],
				specsCurr:0,
				getBefore:{},
				mainData:[],
				typeData:[],
				currId:-1,
				order:{},
				orderItem:'distance',
				orderType:'asc'
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.getBefore =  {
				caseData: {
					tableName: 'Label',
					searchItem: {
						title: ['=', ['静音发电机']],
					},
					middleKey: 'category_id',
					key: 'id',
					condition: 'in',
				},
			};
			self.$Utils.loadAll(['getTypeData','getLocation'], self);
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
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
					if(self.curr==1){
						self.currId = -1;
						self.getBefore =  {
							caseData: {
								tableName: 'Label',
								searchItem: {
									title: ['=', ['静音发电机']],
								},
								middleKey: 'category_id',
								key: 'id',
								condition: 'in',
							},
						};
					}else if(self.curr==2){
						self.currId = self.typeData[0].id;
						self.getBefore = {}
					}
					self.getMainData(true)
				}
			},
			
			changeSpecsCurr(id){
				const self = this;
				if(self.currId!=id){
					self.currId = id;
					self.getMainData(true)
				}
			},
			
			getTypeData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['油机发电机']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.typeData.push.apply(self.typeData, res.info.data);
					}
					console.log('self.typeData', self.typeData)
					self.$Utils.finishFunc('getTypeData');
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
						pagesize: 10
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					check_status:1,
					type:1
				};
				if (JSON.stringify(self.order) != '{}') {
					postData.order = self.$Utils.cloneForm(self.order);
				};
				if (JSON.stringify(self.getBefore) != '{}') {
					postData.getBefore = self.$Utils.cloneForm(self.getBefore);
				};
				if (self.currId>0) {
					postData.searchItem.category_id = self.currId;
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
	}
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.orderNav{position: fixed;top: 0rpx;left: 0;right: 0; z-index: 22;}
	.orderNav .tt{width: 50%;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .rr{width: 66%;}
	
	.leftNav{width: 165rpx;line-height: 96rpx;position: fixed;top:110rpx;bottom: 0;left: 0;z-index: 1;background: #fff;}
	.leftNav .item{border-bottom: 1px solid #F5F5F5;}
	.leftNav .item.on{background: #0094DC; color: #fff;position: relative;}
	.leftNav .item.on:after{content: '';position: absolute; top: 50%;right:-10rpx;transform: translateY(-50%);border-bottom: 6px solid transparent;border-top: 6px solid transparent;border-left: 6px solid #0094DC;}
	
	.R-proList{margin-left:186rpx;margin-right: 4%;}
	.R-proList .proRowList .rr{width: 56%;}
</style>
