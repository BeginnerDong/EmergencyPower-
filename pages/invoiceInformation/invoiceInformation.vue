<template>
	<view class="invoice">
		<view class="flex pdt20 pdb15 pdl10 pdr10 ivohead">
			<view>客户类型</view>
			<view class="flex pdl30" @click="change(1)">
				<image class="ivoIcon" :src="current==1?'../../static/images/icon.png':'../../static/images/icon1.png'" mode=""></image>
				<text>企业单位</text>
			</view>
			<view class="flex pdl25" @click="change(2)">
				<image class="ivoIcon" :src="current==2?'../../static/images/icon.png':'../../static/images/icon1.png'" mode=""></image>
				<text>个人/非企业</text>
			</view>
		</view>
		
		<view class="ivoCon mgl10 mgr10" v-show="current == 2">
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" v-model="submitData.receipt_title" placeholder="请输入姓名" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" v-model="submitData.contact" placeholder="请输入电话" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" v-model="submitData.idcard" placeholder="请输入身份证号" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" v-model="submitData.email" placeholder="请输入邮箱" class="itemInp"/>
			</view>
		</view>
		
		<view class="ivoCon mgl10 mgr10" v-show="current == 1">
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" v-model="submitDataTwo.receipt_title" placeholder="请输入企业名称" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text"  v-model="submitDataTwo.tax_no"  placeholder="请输入企业税号" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text"  v-model="submitDataTwo.registered_address" placeholder="请输入企业注册地址" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" v-model="submitDataTwo.contact"  placeholder="请输入企业联系方式" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" v-model="submitDataTwo.email" placeholder="请输入邮箱" class="itemInp"/>
			</view>
		</view>
		
		<view class="flexRowAround ivoBtn">
			<view class="btn mgl10" @click="noConfirm()">不开发票</view>
			<view class="btn mgr10" @click="confirm()">确定</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				current:1,
				submitData:{
					idcard:'',	
					contact :'',
					receipt_title:'', 
					email:''
				},
				submitDataTwo:{
					tax_no:'',	
					contact :'',
					receipt_title:'', 
					email:'',
					registered_address:''
				}
			}
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('receiptData')){
				if(uni.getStorageSync('receiptData').receipt==1){
					self.submitDataTwo = uni.getStorageSync('receiptData')
				}else if(uni.getStorageSync('receiptData').receipt==2){
					self.submitData = uni.getStorageSync('receiptData')
				}
			}
		},
		
		methods: {
			
			
			noConfirm(){
				const self = this;
				uni.removeStorageSync('receiptData');
				uni.navigateBack({
					delta:1
				})
			},
			
			change(x){
				const self = this;
				self.current = x;
			},
			
			confirm(){
				const self = this;
				if(self.current==1){
					self.submitDataTwo.receipt = self.current;
					const pass = self.$Utils.checkComplete(self.submitDataTwo);
					if(pass){
						uni.setStorageSync('receiptData',self.submitDataTwo);
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					}else{
						self.$Utils.showToast('请补全信息','none');
					}
				}else{
					self.submitData.receipt = self.current;
					const pass = self.$Utils.checkComplete(self.submitData);
					if(pass){
						uni.setStorageSync('receiptData',self.submitData)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					}else{
						self.$Utils.showToast('请补全信息','none');
					}
				}
			},
			
		}
	}
</script>

<style>
	@import "../../assets/style/invoice.css";
	page .invoice{height: 100%;}
</style>
