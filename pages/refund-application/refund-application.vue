<template>
	<view>
		<view class="mt-3 mx-3">
			<view class="proRow ">
				<view class="item mb-3 bg-white" v-for="(item,index) in mainData.child" :key="index">
					<view class="font-24 d-flex j-sb a-center mb-2">
						<view class="color9">交易时间：{{item.create_time}}</view>
						<view class="red" v-if="item.type==1">待发货</view>
						<view class="red" v-if="item.type==6">待核销</view>
					</view>
					<view class="d-flex a-center j-sb">
						<view class="pic" v-if="item.type==1">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product&&
							item.orderItem[0].snap_product.product.mainImg&&item.orderItem[0].snap_product.product.mainImg[0]?item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="pic" v-if="item.type==6">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product&&
							item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="tit avoidOverflow"  v-if="item.type==1">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product?item.orderItem[0].snap_product.product.title:''}}</view>
							<view class="tit avoidOverflow"  v-if="item.type==6">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
							<view class="d-flex font-24 color6 mt-1">
								<view class="specsBtn mr-1"  v-if="item.type==1">{{item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product
							&&item.orderItem[0].snap_product?item.orderItem[0].snap_product.title:''}}</view>
							</view>
							<view class="B-price d-flex a-center j-sb">
								<view class="d-flex a-center">
									<view class="price font-30 font-weight mr-2">{{item.unit_price?item.unit_price:''}}</view>
								<!-- 	<view class="font-24">会员</view>
									<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view> -->
								</view>
								<view class="font-26">×{{item.count?item.count:''}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="bg-white rounded10 px-3 py-3">
				<view class="font-26">
					<textarea v-model="submitData.passage1" placeholder="请填写申请退款理由" />
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 120rpx;" @click="orderUpdate()">
			<button class="btn" type="button">确定</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				submitData:{
					passage1:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
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
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					order_step:1,
					passage1:self.submitData.passage1
				};
				postData.searchItem = {
					id:self.id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.Router.reLaunch({route:{path:'/pages/user/user'}})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	textarea{font-size: 26rpx;width: 100%;height:240rpx;padding: 0;}
	.onloadbtn{width: 120rpx;height: 120rpx;}
	.pjPic image{width: 142rpx;height: 142rpx;margin: 20rpx 20rpx 0 0;border-radius: 10rpx;}
	.pjPic image:nth-of-type(4n){margin-right: 0;}
</style>
