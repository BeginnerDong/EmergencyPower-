<template>
	<view>
		<view class="proRowList pdlr4 mgt15">
			
			<view class="item">
				<view class="flexRowBetween pdb10">
					<view class="fs12 color9">交易时间：{{mainData.create_time}}</view>
					<view class="fs12 red">已完成</view>
				</view>
				<view class="flexRowBetween pdb15" v-if="mainData.product_id>0">
					<view class="ll">
						<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
						mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''"></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 fs13">{{mainData.orderItem&&mainData.orderItem[0]&&
						mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.title:''}}</view>
						<view class="Bmny flexRowBetween">
							<view class="price fs14">{{mainData.orderItem&&mainData.orderItem[0]&&
						mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.price:''}}</view>
							<view class="fs10 color6">×1</view>
						</view>
					</view>
				</view>
				<view class="flexRowBetween pdb15" v-if="mainData.sku_id>0">
					<view class="ll">
						<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product.product&&
						mainData.orderItem[0].snap_product.product.mainImg&&mainData.orderItem[0].snap_product.product.mainImg[0]?
						mainData.orderItem[0].snap_product.product.mainImg[0].url:''"></image>
					</view>
					<view class="rr">
						<view class="avoidOverflow2 fs13">{{mainData.orderItem&&mainData.orderItem[0]&&
						mainData.orderItem[0].snap_product.product?mainData.orderItem[0].snap_product.product.title:''}}</view>
						<view style="color:#666;font-size:12px">{{mainData.orderItem&&mainData.orderItem[0]&&
						mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.title:''}}</view>
						<view class="Bmny flexRowBetween">
							<view class="price fs14">{{mainData.orderItem&&mainData.orderItem[0]&&
						mainData.orderItem[0].snap_product.product?mainData.orderItem[0].snap_product.product.price:''}}</view>
							<view class="fs10 color6">×1</view>
						</view>
					</view>
				</view>
				<!-- <view class="flexEnd pdt15 pdb5">共1件商品 合计:￥56</view> -->
			</view>
		</view>
		
		<view class="mglr4 whiteBj radius8 mgt15 pdlr4">
			<view class="pdtb15 flex">
				<view class="mgr10 fs13">星级评价</view>
				<view class="starBox mgr5">
					<view style="margin-right: 10px;width: 40rpx;height: 40rpx;position: relative;" v-for="item in stars">
						<image class="" style="position: absolute;width: 40rpx;height: 40rpx;" :src="submitData.score > item ?(submitData.score-item == 0.5?halfSrc:selectedSrc) : normalSrc" mode="">
							<view class="startItem" style="left:0rpx"  @click="selectLeft(item+0.5)"></view>
							 <view class="startItem" style="left:20rpx"  @click="selectRight(item+1)"></view>
						</image>
					</view>
					<!-- <image src="../../static/images/details-icon1.png" mode=""></image>
					<image src="../../static/images/details-icon.png" mode=""></image>
					<image src="../../static/images/details-icon.png" mode=""></image>
					<image src="../../static/images/details-icon.png" mode=""></image>
					<image src="../../static/images/details-icon.png" mode=""></image> -->
				</view>
				
			</view>
			<view class="pdtb15 flexRowBetween" style="align-items: flex-start;">
				<view class="fs13  mgt5">填写评价</view>
				<view style="width: 80%;">
					<textarea class="fs12 f5bj radius8" v-model="submitData.description"  style="height: 240rpx;"  placeholder="请写下您的宝贵意见" />
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<button class="btn"   @click="Utils.stopMultiClick(submit)">确定</button>
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
				wx_info:{},
				is_show:false,
				normalSrc: '../../static/images/details-icon.png',
				selectedSrc: '../../static/images/details-icon1.png',
				halfSrc: '../../static/images/details-icon1-a.png',
				submitData: {
					order_no: '',
					relation_id: '',
					score: 0,
					description: '',
					type:1,
					title:'',
					mainImg:[]
				},
				stars: [0, 1, 2, 3, 4],
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			var res = self.$Token.getProjectToken(function(){
				self.$Utils.loadAll(['getMainData'], self)
			});
			if(res){
				self.$Utils.loadAll(['getMainData'], self)
			};
			self.submitData.title = uni.getStorageSync('user_info').nickname
			self.submitData.mainImg = [{type:'image',url:uni.getStorageSync('user_info').headImgUrl}]
		},
		
		methods: {
			
			selectLeft(key) {
			    const self = this;
				console.log('self.submitData.score',self.submitData.score);
				console.log('key',key);
			    if (self.submitData.score == 0.5 && key == 0.5) {
			      self.submitData.score = 0;
			    }else{
					self.submitData.score = key
				}				
			    console.log("得" + self.submitData.score + "分")	 
				console.log(self.submitData)	 
			  },
			  
			
			  selectRight(key) {
				const self = this;
				self.submitData.score = key
			    console.log("得" + key + "分")
			  },
			
					
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.submitData.order_no = self.mainData.order_no;
						if(self.mainData.product_id>0){
							self.submitData.relation_id = self.mainData.product_id
						}else if(self.mainData.sku_id>0){
							self.submitData.relation_id = self.mainData.orderItem[0].snap_product.product.id;
						}
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
					
				};
				self.$apis.orderGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					self.messageAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [{
				  tableName:'Order',
				  FuncName:'update',
				  searchItem:{
				    id:self.id
				  },
				  data:{
				    isremark:1,
				  }
				}];
				const callback = (data) => {
			
					if (data.solely_code == 100000) {
			
						self.$Utils.showToast('评价成功', 'none')
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
			
				};
				self.$apis.messageAdd(postData, callback);
			},
		}
	};
</script>

<style>
	
	
	@import "../../assets/style/proList.css";
	@import "../../assets/style/star.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.proRowList .item{padding: 20rpx;margin-bottom: 30rpx;border-bottom: 0;border-radius: 10rpx;}
	.proRowList .item .ll{width: 160rpx; height: 160rpx;}
	.proRowList .rr{width: 72%;height: 160rpx;}
	.starBox image{width: 28rpx;height: 28rpx;margin-right: 8rpx;}
	.star-image {
	  position: absolute;
	  width: 40rpx;
	  height: 40rpx;
	  src: "../../images/home-icon11.png";
	}
	 
	.startItem {
	  position: absolute;
		top:0;
	  width: 20rpx;
	  height: 40rpx;
	}
</style>
