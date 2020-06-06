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
				<input type="text" placeholder="请输入姓名" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" placeholder="请输入电话" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" placeholder="请输入身份证号" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" placeholder="请输入邮箱" class="itemInp"/>
			</view>
		</view>
		
		<view class="ivoCon mgl10 mgr10" v-show="current == 1">
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" placeholder="请输入企业名称" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" placeholder="请输入企业税号" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" placeholder="请输入企业注册地址" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" placeholder="请输入企业联系方式" class="itemInp"/>
			</view>
			<view class="pdt20 pdb20 item pdl15">
				<input type="text" placeholder="请输入邮箱" class="itemInp"/>
			</view>
		</view>
		
		<view class="flexRowAround ivoBtn">
			<view class="btn mgl10">不开发票</view>
			<view class="btn mgr10">确定</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				current:1
			}
		},
		methods: {
			change(x){
				const self = this;
				self.current = x;
			}
		}
	}
</script>

<style>
	@import "../../assets/style/invoice.css";
	page .invoice{height: 100%;}
</style>
