<template>
	<view>
		
		<view>
			<view class="myRowBetween fs13">
				<view class="item flexRowBetween">
					<view class="ll">商品名称</view>
					<view class="rr">
						<input type="text" v-model="submitData.title" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">商品价格</view>
					<view class="rr">
						<input type="text" v-model="submitData.price" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween" v-if="level>=1">
					<view class="ll">选择类别</view>
					<view class="rr">
						<!-- <view class="color9">请选择</view> -->
						<picker mode="selector" :range="labelData" range-key="title" 
						@change="index1Change">
							{{labelData[index1].title?labelData[index1].title:'请选择'}}
						</picker>
						<!-- <lb-picker ref="picker1"
							:level="3"
						  mode="multiSelector"
						  :list="array"
						  :props="props"
						  @change="handleChange"
						  @confirm="handleConfirm"
						  @cancle="handleCancle">
						</lb-picker> -->
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				
				<view class="item flexRowBetween" v-if="level>=2">
					<view class="ll">选择类别</view>
					<view class="rr">
						<!-- <view class="color9">请选择</view> -->
						<picker mode="selector" :range="labelTwoData" range-key="title" 
						@change="index2Change">
							{{labelTwoData[index2].title?labelTwoData[index2].title:'请选择'}}
						</picker>
						<!-- <lb-picker ref="picker1"
							:level="3"
						  mode="multiSelector"
						  :list="array"
						  :props="props"
						  @change="handleChange"
						  @confirm="handleConfirm"
						  @cancle="handleCancle">
						</lb-picker> -->
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">库存</view>
					<view class="rr">
						<input type="text" v-model="submitData.stock" placeholder="请输入" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">所在地</view>
					<view class="rr" @click="chooseAddress()">
						<input type="text" placeholder="选择位置" disabled="true"   v-model="submitData.passage1">
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item">
					<view class="mgr15">详情介绍</view>
					<view>
						<textarea style="height: 200rpx;border: 0;padding:20rpx 0;" v-model="submitData.content" placeholder="请输入" />
					</view>
					<view class="flex upPicBox xq-upImg">
						<view class="list" v-for="item in submitData.contentImg" style="position: relative;">
							<image class="pic" :src="item.url" mode=""></image>
							<image @click="deleteImg(index,'contentImg')" src="../../static/images/deleteImg.png" mode=""
							style="width: 35rpx;height: 35rpx;position: absolute;top: -17.5rpx;right:-17.5rpx;"></image>
						</view>
						<view class="list" @click="upLoadImg('contentImg')">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image>
						</view>
						
					</view>
				</view>
				<view class="item flex">
					<view style="width: 20%;">商品主图</view>
					<view class="flex upPicBox" style="width: 80%;" >
						<view class="list" v-for="item in submitData.mainImg" style="position: relative;">
							<image class="pic" :src="item.url" mode=""></image>
							<image @click="deleteImg(index,'mainImg')" src="../../static/images/deleteImg.png" mode=""
							style="width: 35rpx;height: 35rpx;position: absolute;top: -17.5rpx;right:-17.5rpx;"></image>
						</view>
						<view class="list" @click="upLoadImg('mainImg')" v-if="submitData.mainImg.length==0">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image>
						</view>
						
					</view>
				</view>
				<view class="item flex">
					<view style="width: 20%;">轮播图</view>
					<view class="flex upPicBox lunbo-upImg" style="width: 80%;">
						<view class="list" v-for="item in submitData.bannerImg" style="position: relative;">
							<image class="pic" :src="item.url" mode=""></image>
							<image @click="deleteImg(index,'bannerImg')" src="../../static/images/deleteImg.png" mode=""
							style="width: 35rpx;height: 35rpx;position: absolute;top: -17.5rpx;right:-17.5rpx;"></image>
						</view>
						<view class="list" @click="upLoadImg('bannerImg')">
							<image class="jiaBtn" src="../../static/images/add-icon.png" mode=""></image>
							
						</view>
						
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 120rpx;">
				<button class="btn" type="button" @click="submit()">确定</button>
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
				currSex:0,
				labelData:[],
				array:[],
				skuLabelData:[],
				level:0,
				index1:-1,
				index2:-1,
				labelTwoData:[],
				submitData:{
					title:'',
					category_id:'',
					stock:'',
					longitude:'',
					latitude:'',
					content:'',
					contentImg:[],
					mainImg:[],
					bannerImg:[],
					passage1:'',
					type:1,
					price:''
				},
				skuLabelId:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.label_id = options.label_id;
			
			console.log('options.label_id',options.label_id)
			if(options.id){
				self.id = options.id;
				self.$Utils.loadAll(['getMainData'], self);
			}
			self.$Utils.loadAll(['getSkuLabelData'], self);
		},
		
		methods: {
			
			deleteImg(index,type){
				const self = this;
				self.submitData[type].splice(parseInt(index), 1);
			},
			
			getSkuLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:['in',[5]]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.skuLabelData.push.apply(self.skuLabelData,res.info.data);
						for (var i = 0; i < self.skuLabelData.length; i++) {
							self.skuLabelId.push(self.skuLabelData[i].id)
						}
					} 
					self.getLabelData()
					
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				//postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					id:self.id
				};
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.submitData.title = res.info.data[0].title;
						self.submitData.category_id = res.info.data[0].category_id;
						self.submitData.stock = res.info.data[0].stock;
						self.submitData.longitude = res.info.data[0].longitude;
						self.submitData.latitude = res.info.data[0].latitude;
						self.submitData.content = res.info.data[0].content;
						self.submitData.contentImg = res.info.data[0].contentImg;
						self.submitData.mainImg = res.info.data[0].mainImg;
						self.submitData.bannerImg = res.info.data[0].bannerImg;
						self.submitData.passage1 = res.info.data[0].passage1;
						self.submitData.price = res.info.data[0].price;
					} else {
						self.$Utils.showToast(res.msg,'success')
					}
				};
				self.$apis.productGet(postData, callback);
			},
			
			productAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('添加成功','success');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'success')
					}
				};
				self.$apis.productAdd(postData, callback);
			},
			
			productUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.searchItem= {
					id:self.id
				};
				postData.data.check_status = 0;
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('编辑成功,需平台重新审核','success');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'success')
					}
				};
				self.$apis.productUpdate(postData, callback);
			},
			
			
			submit() {
				const self = this;
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					if(self.id){
						self.productUpdate();
					}else{
						self.productAdd();
					}
					
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息','success');
				};
			},
			
			upLoadImg(type) {
				const self = this;			
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
			
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						uni.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getThirdToken',
							
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			chooseAddress(e) {
				const self = this;
				uni.authorize({
				    scope: 'scope.userLocation',
				    success() {
				        uni.chooseLocation({
				        	success: (res) => {
				        		console.log(res)
				        		self.submitData.passage1 = res.address;
				        		self.submitData.longitude = res.longitude;
				        		self.submitData.latitude  = res.latitude;
				        	},
				        	fail: (e) => {
				        		uni.getSetting({
				        			success: (res) => {
				        				console.log(res)
				        				let locaAuth = res.authSetting['scope.userLocation']
				        				if (locaAuth) {/* 判断位置是否已经授权，是选择地图位置点击取消触发的fail，再选择位置 */
				        					console.log('地图点击取消')
				        					uni.chooseLocation({
				        						success: (res) => {
				        							self.submitData.passage1 = res.address;
				        							self.submitData.longitude = res.longitude;
				        							self.submitData.latitude  = res.latitude;
				        						},
				        					});
				        				}
				        				if (!locaAuth) { /* 如果地理位置没授权 */
				        					console.log(222)
				        					uni.showModal({
				        					    title: '提示',
				        					    content: '需要授权位置信息',
				        						confirmColor:'#ca1c1d',
				        						showCancel:true,
				        					    success: function (res) {
				        					        if (res.confirm) {
				        					            uni.openSetting({
				        					            	success: (res) => {
				        					            		console.log(res.authSetting)
				        					            	},
				        					            	fail: (res) => {
				        					            		console.log(res)
				        					            	},
				        					            });
				        					        } else if (res.cancel) {
				        					           
				        					        }
				        					    }
				        					});			
				        				}
				        			}
				        		})
				        	}
				        });
				    },
					fail: (e) => {
						uni.showModal({
						    title: '提示',
						    content: '需要授权位置信息',
							confirmColor:'#ca1c1d',
							showCancel:true,
						    success: function (res) {
						        if (res.confirm) {
						            uni.openSetting({
						            	success: (res) => {
						            		console.log(res.authSetting)
						            	},
						            	fail: (res) => {
						            		console.log(res)
						            	},
						            });
						        } else if (res.cancel) {
						           
						        }
						    }
						});
					}
				})
				
			},
			
			index1Change(e){
				const self = this;
				console.log('picker发送选择改变，携带值为', e.target.value)
				self.index1 = e.target.value;
				self.index1Id = self.labelData[self.index1].id;
				self.getLabelTwoData()
			},
			
			index2Change(e){
				const self = this;
				console.log('picker发送选择改变，携带值为', e.target.value)
				self.index2 = e.target.value;
				self.submitData.category_id = self.labelTwoData[self.index2].id;
			},
			
			changeSex(currSex){
				const self = this;
				if(currSex!=self.currSex){
					self.currSex = currSex
				}
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					parentid:self.label_id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData,res.info.data);
						self.level = 1;
					}else{
						self.submitData.category_id = self.label_id;
						self.submitData.sku_array = self.skuLabelId;
					}
					self.$Utils.finishFunc('getSkuLabelData');
					//console.log('self.array', self.array)
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getLabelTwoData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					parentid:self.index1Id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelTwoData.push.apply(self.labelTwoData,res.info.data);
						self.level = 2;
					}else{
						self.level = 1;
						self.submitData.category_id = self.labelData[self.index1].id;
					}
					self.$Utils.finishFunc('getLabelData');
					//console.log('self.array', self.array)
				};
				self.$apis.labelGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	
	.myRowBetween .item{padding: 30rpx 4%; align-items: flex-start;}
	.person-phtoto{width: 60rpx;height: 60rpx;border-radius: 50%;background: #e1e1e1;overflow: hidden;}
	.person-phtoto image{width: 100%;height: 100%;display: block;border-radius: 50%;}
	
	.seltIcon{width: 36rpx;height: 36rpx; display: block;margin-right: 10rpx;}
	
	.upPicBox{flex-wrap: wrap;}
	.upPicBox .list{width: 180rpx;height:180rpx;margin:0 20rpx 20rpx 0;border-radius: 8rpx;position: relative;background: #F5F5F5;}
	.upPicBox .list image{width: 100%;height: 100%;display: block;}
	.upPicBox .list image.jiaBtn{width:50rpx;height: 50rpx;display: block;position:absolute;top:50%;left: 50%;transform: translate(-50%,-50%);}
	
	.xq-upImg .list{width: 200rpx; height: 140rpx;}
	.upImg{width: 200rpx; height: 230rpx;background: #F5F5F5;}
	
	.lunbo-upImg .list{width: 300rpx;height: 152rpx;}
	
</style>

