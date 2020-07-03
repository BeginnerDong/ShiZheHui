<template>
	<view>
		<view class="jg"></view>
		<view class="orderHead">
			<image src="../../static/images/icon-01.png" mode=""></image>
			<view class="bg-white p-3 font-28 color2">
				<view class="d-flex a-center pb-1">
					<view>{{mainData.snap_address?mainData.snap_address.name:''}}</view>
					<view class="color9 font-24 pl-3">{{mainData.snap_address?mainData.snap_address.phone:''}}</view>
				</view>
				<view>{{mainData.snap_address?mainData.snap_address.city+mainData.snap_address.detail:''}}</view>
			</view>
		</view>
		
		<view class="jg"></view>
		<view class="bg-white">
			<view class="pt-3">
				<view class="d-flex a-center j-sb px-3 pb-3" v-for="item of mainData.child" :key="item.id">
					<view class="pic pr-2">
						<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&item.orderItem[0].snap_product.product&&
						item.orderItem[0].snap_product.product.mainImg&&item.orderItem[0].snap_product.product.mainImg[0]?item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor flex-1 d-flex flex-column j-sb">
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
			<view class="line-h py-3 border-top d-flex font-28 color2 j-sb mx-3">
				<view>已支付</view>
				<view class="d-flex">
					<view class="font-27 color6">共{{mainData.child?mainData.child.length:''}}件商品</view>
					<view class="font-30 pl-2 font-weight price1">{{mainData.price}}</view>
				</view>
			</view>
			<view class="line-h py-3 border-top d-flex font-28 color2 j-sb mx-3">
				<view>订单编号</view>
				<view class="font-27 color6">{{mainData.order_no}}</view>
			</view>
		</view>
		
		<view class="jg"></view>
		<view class="bg-white pb-3">
			<view class="font-30 color2 pt-4 font-weight pl-3 line-h">物流信息</view>
			<view class="shadow font-26 color2 p-3 mx-3 mt-3 d-flex a-center j-sb wl" 
			v-if="expressData.length>0" v-for="item of expressData" :key="item.id">
				<view class="wlTxt">
					<view>物流公司：{{item.express?item.express.title:''}}</view>
					<view class="pt-3">物流单号：{{item.result}}</view>
					<view class="pt-3">{{item.content}}</view>
				</view>
				<view 
					:data-no = "item.result"
					:data-code="item.express.description"
					:data-id = "item.id"
					
					@click="Router.navigateTo({route:{path:'/pages/userOrder-wuliu/userOrder-wuliu?no='
					+$event.currentTarget.dataset.no+'&code='
					+$event.currentTarget.dataset.code+'&id='
					+$event.currentTarget.dataset.id+'&orderId='
					+mainData.id}})">
					<image src="../../static/images/home-icon7.png"></image>
				</view>
			</view>
			<view v-if="expressData.length==0" style="width: 100%;text-align: center;margin-top: 50px;">暂无物流信息</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				expressData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
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
						self.getExpressData()
					};
				};
				self.$apis.orderGet(postData, callback);
			},
			
			getExpressData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					relation_id:self.mainData.id
				};
				postData.getAfter = {
					express:{
						tableName:'Label',
						middleKey:'title',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['title','description']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.expressData.push.apply(self.expressData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			
			
		}
	};
</script>

<style>
.jg{background-color: #f5f5f5;height: 20rpx;width: 100%;}
.orderHead image{width: 100%;height: 8rpx;}
.pic image{width: 160rpx;height: 160rpx;}
.infor{height: 160rpx;}
.specsBtn{background-color: #f5f5f5;}
.price1{color: #ed0000;}
.price1::before{content: '￥';}
.wl image{width: 20rpx;height: 30rpx;margin: 30rpx 0 30rpx 50rpx;}
.wlTxt{width: 460rpx;}
</style>
