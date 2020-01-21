<template>
	<view>
		<view class="myRowBetween fs13">
			<view class="item flexRowBetween">
				<view class="ll">规格名称</view>
				<view class="rr"><input type="text" v-model="submitData.title" placeholder="请输入" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">价格</view>
				<view class="rr"><input type="text" v-model="submitData.price" placeholder="请输入" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">库存</view>
				<view class="rr"><input type="text" v-model="submitData.stock" placeholder="请输入" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">{{labelData[0].title}}</view>
				<view class="rr">
					<picker mode="selector" :range="labelData[0].child" range-key="title" 
					@change="bindChangeOne" :data-item="labelData[0].id">
						<view>{{labelData[0].child[indexOne]?labelData[0].child[indexOne].title:'请选择'}}</view>
					</picker>
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">{{labelData[1].title}}</view>
				<view class="rr">
					<picker mode="selector" :data-item="labelData[1].id" :range="labelData[1].child" range-key="title" @change="bindChangeTwo">
						<view>{{labelData[1].child[indexTwo]?labelData[1].child[indexTwo].title:'请选择'}}</view>
					</picker>
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top: 400rpx;">
			<button class="btn" type="button" @click="submit">确定</button>
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
				labelData:[],
				indexOne:-1,
				indexTwo:-1,
				submitData:{
					title:'',
					price:'',
					stock:'',
					sku_item:[],
					product_no:''
				},
				test:{
					
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			
			self.submitData.product_no = options.product_no;
			
			self.$Utils.loadAll(['getLabelData'], self);
			if(options.id){
				self.id = options.id;
				self.$Utils.loadAll(['getMainData'], self);
			};
		},
		
		methods: {
			
			submit(){
				const self = this;
				for(var k in self.test)
				{
					self.submitData.sku_item.push(self.test[k])
				}
				console.log('self.submitData.sku_item',self.submitData.sku_item)
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('self.data.sForm', self.submitData)
				console.log('pass', pass)
				if (pass) {
					self.skuAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息','success');
				};
			},
			
			skuAdd() {
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
				self.$apis.skuAdd(postData, callback);
			},
			
			
		
			
			bindChangeOne: function(e) {
				const self = this;
			    console.log('picker发送选择改变，携带值为', e);
				self.indexOne = e.detail.value;
				for(let key  in self.test){
					if(key==e.currentTarget.dataset.item){
						delete self.test[e.currentTarget.dataset.item]
					}
				}
				self.test[e.currentTarget.dataset.item] = self.labelData[0].child[e.detail.value].id
				console.log(self.test)
			  },
			  
			  bindChangeTwo: function(e) {
			  	const self = this;
			     console.log('picker发送选择改变，携带值为', e);
			  	self.indexTwo = e.detail.value;
			  	for(let key  in self.test){
			  		if(key==e.currentTarget.dataset.item){
			  			delete self.test[e.currentTarget.dataset.item]
			  		}
			  	}
			  	self.test[e.currentTarget.dataset.item] = self.labelData[1].child[e.detail.value].id
			  	console.log(self.test)
			  },
			
			/* bindChangeOne(e){
				const self = this;
				console.log
				self.submitData.sku_item.push({item:self.labelData[0].child[e.detail.value].id});
				console.log(self.submitData.sku_item)
			}, */
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:['in',[5,6]]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData,res.info.data)
					} 
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};
				postData.getAfter = {
					label:{
						tableName:'Label',
						middleKey:'sku_item',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.title = self.mainData.title;
						self.submitData.stock = self.mainData.stock;
						self.submitData.sku_item = self.mainData.sku_item;
						self.submitData.product_no = self.mainData.product_no;
						self.submitData.price = self.mainData.price;
						for (var i = 0; i < self.labelData[0].child.length; i++) {
							for (var j = 0; j < self.mainData.label.length; j++) {
								if(self.labelData[0].child[i].id==self.mainData.label[j].id){
									self.indexOne = i
									self.test[self.mainData.label[j].parentid] = self.mainData.label[j].id
								}
							}
						}
						for (var i = 0; i < self.labelData[1].child.length; i++) {
							for (var j = 0; j < self.mainData.label.length; j++) {
								if(self.labelData[1].child[i].id==self.mainData.label[j].id){
									self.indexTwo = i
									self.test[self.mainData.label[j].parentid] = self.mainData.label[j].id
								}
							}
						}
					} 
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.skuGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 60rpx;}
	
	.myRowBetween .item{padding: 30rpx 4%;}
</style>

