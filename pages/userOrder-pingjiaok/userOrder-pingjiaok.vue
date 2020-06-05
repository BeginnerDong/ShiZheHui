<template>
	<view>
		
		<view class="mx-3">
			<view class="proRow">
				<view class="item mt-3 bg-white"  v-for="(item,index) in mainData" :key="index">
					<view>
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red">已完成</view>
						</view>
						<view class="d-flex a-center j-sb">
							<view class="pic" v-if="item.type==1">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
								c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="pic" v-if="item.type==6">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product&&
								c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow"  v-if="item.type==1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product?c_item.orderItem[0].snap_product.product.title:''}}</view>
								<view class="tit avoidOverflow"  v-if="item.type==6">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								<view class="d-flex font-24 color6 mt-1">
									<view class="specsBtn mr-1"  v-if="item.type==1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="d-flex a-center">
										<view class="price font-30 font-weight mr-2">{{c_item.unit_price?c_item.unit_price:''}}</view>
										<!-- <view class="font-24">会员</view>
										<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view> -->
									</view>
									<view class="font-26">×{{c_item.count?c_item.count:''}}</view>
								</view>
							</view>
						</view>
					</view>
					<view class="f5Text f5bj rounded10 p-2 font-26 color6 mt-2">
						<view>{{item.message&&item.message[0]?item.message[0].description:''}}</view>
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
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.no = options.no;
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
				postData.searchItem = {
					parent_no: self.no,
				};
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
				postData.getAfter= {
					message:{
						tableName:'Message',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'=',
					},
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'=',
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.pusha.apply(self.mainData,res.info.data)
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
	page{background: #F5F5F5;padding-bottom:30rpx;}
	
	.f5Text{line-height: 42rpx;}
</style>
