<template>
	<view>
		
		<view class="topNavFix bg-white">
			<view class="d-flex a-center px-3 pt-2 pb-1">
				<view class="photo overflow-h"><image :src="userData.mainImg&&userData.mainImg[0]?userData.mainImg[0].url:''" mode=""></image></view>
				<view class="ml-2 font-30">{{userData.info?userData.info.name:''}}</view>
			</view>
			<view class="orderNav bg-white d-flex j-sb a-center shadow color6">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">待核销</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已核销</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view class="mt-3 mx-3">
			<view class="proRow">
				<view class="item mb-3" v-for="(item,index) in mainData" :key="index">
					<view class="font-24 d-flex j-sb a-center mb-2">
						<view class="color9">交易时间：{{item.create_time}}</view>
						<view class="red">{{item.transport_status==0?'待核销':'已核销'}}</view>
					</view>
					<view class="priList pb-3">
						<view class="d-flex a-center j-sb">
							<view class="pic">
								<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product&&
							item.orderItem[0].snap_product.product.mainImg&&item.orderItem[0].snap_product.product.mainImg[0]?item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product?item.orderItem[0].snap_product.product.title:''}}</view>
								<view class="d-flex font-24 color6 mt-1">
									<view class="specsBtn mr-1">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
								</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="d-flex a-center">
										<view class="price font-30 font-weight mr-2">{{item.unit_price?item.unit_price:''}}</view>
										<!-- <view class="font-24">会员</view>
										<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view> -->
									</view>
									<view class="font-26">×{{item.count?item.count:''}}</view>
								</view>
							</view>
						</view>
					</view>
					<view class="pb-3 d-flex a-center j-sb font-26 pt-3 border-top">
						<view>客户信息：</view>
						<view class="d-flex a-center"><span class="mr-2">{{item.name}}</span>{{item.phone}}</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="R-fixIcon"><image src="../../static/images/merchantsl-icon.png" mode="" @click="scanCode()"></image></view>
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
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
				is_show:false,
				curr:1,
				orderData:2,
				userData:{},
				searchItem: {
					pay_status: 1,
					transport_type: 2,
					type: 1,
					level:0,
					user_type:0
				},
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserData'], self);
		},
		
		onShow() {
			const self = this;
			self.getMainData(true)
		},
		
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
					if (self.curr == 1) {
						delete self.searchItem.transport_status
					} else if (self.curr == 2) {
						self.searchItem.transport_status = 0
					} else if (self.curr == 3) {
						self.searchItem.transport_status = 2
					}
					self.getMainData(true)
				}
			},
			
			scanCode() {
				const self = this;
				uni.scanCode({
					success: function(res) {
						self.Router.navigateTo({
							route: {
								path: '/pages/storeOrder-detail/storeOrder-detail?id=' + res.result
							}
						})
						console.log('条码内容：' + res.result);
					}
				});
			},
			
			getUserData() {
				const self = this;		
				const postData = {};
				var nowTime = (new Date()).getTime() / 1000;
				postData.tokenFuncName = 'getMerchantToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];
						
					}
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
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
				postData.tokenFuncName = 'getMerchantToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
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
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 220rpx;position: fixed;top:0;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height: 220rpx;}
	
	.proRow .item{padding: 20rpx 30rpx 0 30rpx;}
	.orderNav .tt.on::after{bottom: 0;}
	
	.photo{width: 110rpx;height: 110rpx;border-radius: 50%;background-color: #eee;}
</style>
