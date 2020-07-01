<template>
	<view>
		<view class="bg-white mx-3 my-2 rounded10 font-26 color2 p-3">
			<view>物流公司：{{expressData.express?expressData.express.title:''}}</view>
			<view class="pt-3 line-h">物流单号：{{expressData.result?expressData.result:''}}</view>
			<view class="pt-3">{{expressData.content?expressData.content:''}}</view>
		</view>
		
		<view class="bg-white rounded pt-5 pb px-3 mb-5 mx-3">
			<view class="d-flex a-center j-sb">
				<view class="time"></view>
				<view class="font-26 shou text-center rounded-circle mr-2">收</view>
				<view class="font-26 color2 gps avoidOverflow2">{{orderData.snap_address?orderData.snap_address.city+orderData.snap_address.detail:''}}</view>
			</view>
			<view class="d-flex a-center j-sb line-h my-5" v-for="(item,index) of mainData.Traces" :key="index">
				<view class="time text-center">
					<view class="font-24 color2 pb-1">{{item.date}}</view>
					<view class="font-24 color6">{{item.time}}</view>
				</view>
				<view class="point rounded-circle mr-2 position-relative"></view>
				<view class="font-26 color6 gps avoidOverflow">{{item.AcceptStation}}</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				mainData:{},
				expressData:{},
				orderData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.no = options.no;
			self.code = options.code;
			self.id = options.id;
			self.orderId = options.orderId;
			self.$Utils.loadAll(['getMainData','getExpressData','getOrderData'], self);
		},
		
		methods: {
			
			getOrderData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.orderId,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.orderData = res.info.data[0];
					};
					self.$Utils.finishFunc('getOrderData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			getExpressData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
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
						self.expressData = res.info.data[0]
					}
					self.$Utils.finishFunc('getExpressData');
				};
				self.$apis.logGet(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {
					tokenFuncName:'getProjectToken',
					ShipperCode:self.code,
					LogisticCode:self.no
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.mainData = res.info;
						for (var i = 0; i < self.mainData.Traces.length; i++) {
							self.mainData.Traces[i].date = self.mainData.Traces[i].AcceptTime.substr(5,6)
							self.mainData.Traces[i].time = self.mainData.Traces[i].AcceptTime.substr(11,5)
						}
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.getExpress(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}

.shou{width: 50rpx;height: 50rpx;line-height: 50rpx;background-color: #FFBDDA;color: #fff;z-index: 2;}
.time{width: 100rpx;}
.point{width: 14rpx;height: 14rpx;background-color: #dcdddc;}
.gps{width: 460rpx;}
.point::before{content: '';height: 110rpx;width: 2rpx;background-color: #e1e1e1;position: absolute;bottom: 0;left: 6rpx;}
</style>
