<template>
	<view>
		
		<view class="pingjia mglr4">
			<view class="pjList pdtb15" v-for="(item,index) in mainData" :key="index" v-if="mainData.length>0">
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
			<view  style="text-align: center;height: 100rpx;line-height: 100rpx;" v-if="mainData.length==0">
				暂无评价~
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
				score:'',
				wx_info:{},
				pingjiaList:[{},{},{},{},{},{}],
				mainData:[],
				stars: [0, 1, 2, 3, 4],
				normalSrc: '../../static/images/details-icon.png',
				selectedSrc: '../../static/images/details-icon1.png',
				halfSrc: '../../static/images/details-icon1-a.png',
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					relation_id :self.id,
					type:1,
					
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
		}
	}
</script>

<style>
	@import "../../assets/style/detial.css";
	@import "../../assets/style/star.css";
	
	page{padding-bottom: 60rpx;}
	
</style>
