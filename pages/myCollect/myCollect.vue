<template>
	<view class="" >
		<view class="orderNav pdlr4 whiteBj flexRowBetween color6">
			<view class="tt" :class="labelId==item.id?'on':''" @click="changeCurr(index+1,item.id)" 
			v-for="(item,index) in labelData">
				{{item.title}}
			</view>

		</view>
		<view class="pdtb20 mgb15"></view>
		<view class="mglr4" v-show="curr==1">
			<view class="proRowList">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index"
				:data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/pageDetail/pageDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="ll">
						<image :src="item.product&&item.product[0]&&item.product[0].mainImg&&item.product[0].mainImg[0]?
						item.product[0].mainImg[0].url:''"></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 fs13">{{item.product&&item.product[0]?item.product[0].title:''}}</view>
						<view class="fs10 color9">{{item.product&&item.product[0]?item.product[0].description:''}}</view>
						<view class="Bmny flexRowBetween">
							<view class="price fs14">{{item.product&&item.product[0]?item.product[0].price:''}}/小时</view>
							<view class="fs10 color6">销量:{{item.product&&item.product[0]?item.product[0].sale_count:''}}</view>
						</view>
						
					</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4" v-show="curr==2">
			<view class="proLis flexRowBetween">
				<view class="item-lis boxShaow" v-for="(item,index) in mainData" :key="index" @click="Router.navigateTo({route:{path:'/pages/pageDetail/pageDetail'}})">
					<view class="pic">
						<image :src="item.product&&item.product[0]&&item.product[0].mainImg&&item.product[0].mainImg[0]?
						item.product[0].mainImg[0].url:''"></image>
					</view>
					<view class="infor">
						<view class="tit avoidOverflow fs13 pdb5">{{item.product&&item.product[0]?item.product[0].title:''}}</view>
						<view class="flexRowBetween">
							<view class="price">{{item.product&&item.product[0]?item.product[0].price:''}}</view>
							<view class="fs10 color9">销量:{{item.product&&item.product[0]?item.product[0].sale_count:''}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4" v-show="curr==3">
			<view class="proLis flexRowBetween">
				<view class="item-lis boxShaow" v-for="(item,index) in mainData" :key="index" @click="Router.navigateTo({route:{path:'/pages/operatorDetail/operatorDetail'}})">
					<view class="pic">
						<image :src="item.product&&item.product[0]&&item.product[0].mainImg&&item.product[0].mainImg[0]?
						item.product[0].mainImg[0].url:''"></image>
					</view>
					<view class="infor">
						<view class="tit avoidOverflow fs13 pdb5">{{item.product&&item.product[0]?item.product[0].title:''}}</view>
						<view class="flexRowBetween">
							<view class="price">{{item.product&&item.product[0]?item.product[0].price:''}}</view>
							<view class="fs10 color9">销量:{{item.product&&item.product[0]?item.product[0].sale_count:''}}</view>
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
				mechanicsList:[{},{},{},{},{}],
				operatorList:[{},{},{},{},{}],
				labelData:[],
				mainData:[],
				labelId:-1
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getLabelData'], self);
		},
		
		methods: {
			changeCurr(curr,id){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr;
					self.labelId = id
					self.getChildLabel()
				}
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
				};
				postData.order = {
					listorder:'desc'
				}
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
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.labelId
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.idArray = res.info.data
						self.getMainData()
					}
					console.log('self.labelData', self.labelData)
				};
				self.$apis.childLabelGet(postData, callback);
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
					behavior:['in',self.idArray],
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'relation_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.logGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proList.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	.orderNav{position: fixed;top: 0rpx;left: 0;right: 0; z-index: 22;}
	.orderNav .tt{width: 33.33%;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .rr{width: 66%;}
	
</style>
