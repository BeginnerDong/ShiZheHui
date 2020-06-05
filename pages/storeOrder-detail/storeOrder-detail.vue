<template>
	<view>
		
		<view class="mt-3 mx-3">
			<view class="proRow">
				<view class="item mb-3">
					<view class="font-24 d-flex j-sb a-center mb-2">
						<view class="color9">交易时间：{{mainData.create_time}}</view>
						<view class="red">{{mainData.transport_status==0?'待核销':'已核销'}}</view>
					</view>
					<view class="d-flex a-center j-sb">
						<view class="pic" v-if="mainData.type==1">
							<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&mainData.orderItem[0].snap_product.product&&
						mainData.orderItem[0].snap_product.product.mainImg&&mainData.orderItem[0].snap_product.product.mainImg[0]?mainData.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="pic" v-if="mainData.type==6">
							<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&mainData.orderItem[0].snap_product.product&&
						mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="tit avoidOverflow"  v-if="mainData.type==1">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product
							&&mainData.orderItem[0].snap_product&&mainData.orderItem[0].snap_product.product?mainData.orderItem[0].snap_product.product.title:''}}</view>
							<view class="tit avoidOverflow"  v-if="mainData.type==6">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product
							&&mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.title:''}}</view>
							<view class="d-flex font-24 color6 mt-1">
								<view class="specsBtn mr-1">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product
							&&mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.title:''}}</view>
							
							</view>
							<view class="B-price d-flex a-center j-sb">
								<view class="d-flex a-center">
									<view class="price font-30 font-weight mr-2">{{mainData.unit_price?mainData.unit_price:''}}</view>
									<!-- <view class="font-24">会员</view>
									<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view> -->
								</view>
								<view class="font-26">×{{mainData.count?mainData.count:''}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		
			<view class="px-3 py-3 mt-3 d-flex a-center j-sb font-26 bg-white">
				<view>客户信息：</view>
				<view class="d-flex a-center"><span class="mr-2">
				{{mainData.name?mainData.name:''}}</span>{{mainData.phone?mainData.phone:''}}</view>
			</view>
			
		</view>
		
		<view class="submitbtn" style="margin-top: 300rpx;">
			<button class="btn"  type="submint" v-if="mainData.transport_status==0" @click="$Utils.stopMultiClick(orderUpdate)">确认核销</button>
			<button class="btn" type="submint" v-if="mainData.transport_status==2">已核销</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getMerchantToken';
				postData.data = {
					transport_status:2,
				};
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{
						id:self.id,
						user_type:0
					}
				};
				postData.tokenFuncName = 'getMerchantToken'
				postData.searchItem.shop_no = uni.getStorageSync('merchantInfo').user_no;
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1,
							user_type:0
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	
	page{background: #F5F5F5;padding-bottom: 120rpx;}
	
	
	
	.samllEwm{width: 78rpx;height: 78rpx;}
	
	.FixB-btn{position: fixed;left: 0;right: 0;bottom: 0;height: 80rpx;line-height: 80rpx;}
</style>
