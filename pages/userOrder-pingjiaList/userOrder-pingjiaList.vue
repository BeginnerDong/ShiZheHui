<template>
	<view>
		
		<view class="mt-3 mx-3">
			<view class="proRow ">
				<view class="item mb-3 bg-white" v-for="(item,index) in mainData.child" :key="index">
					<view class="font-24 d-flex j-sb a-center mb-2">
						<view class="color9">交易时间：{{item.create_time}}</view>
						<view class="red">已收货</view>
					</view>
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
								<!-- 	<view class="font-24">会员</view>
									<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view> -->
								</view>
								<view class="font-26">×{{item.count?item.count:''}}</view>
							</view>
						</view>
					</view>
					<view class="underBtn d-flex j-end a-center pt-3" v-if="item.isremark==0">
						<view class="Bbtn red" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjia/userOrder-pingjia?id='+$event.currentTarget.dataset.id}})">去评价</view>
					</view>
	
					<view class="underBtn d-flex j-end a-center pt-3" v-if="item.isremark==1">
						<view class="Bbtn">已评价</view>
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
				mainData:{}
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
	page{background: #F5F5F5;}
	
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	
</style>
