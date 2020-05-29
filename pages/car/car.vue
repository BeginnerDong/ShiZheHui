<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="mx-3">
			<view class="py-3 d-flex j-sb a-center">
				<view>全部宝贝({{mainData.length?mainData.length:'0'}})</view>
				<view class="fs13"  v-show="!is_allDelt" @click="allDeltShow">管理</view>
				<view class="fs13"  v-show="is_allDelt" @click="allDeltShow">完成</view>
			</view>
			
			<view class="proRow">
			 	<view class="item d-flex j-sb a-center" v-for="(item,index) in mainData" :key="index">
					<view class="item_selBtn flex">
						<view><image class="seltIcon" src="../../static/images/shopping-icon.png" v-if="item.isSelect" 
						@click="choose(index)"></image>
						<image class="seltIcon" src="../../static/images/shopping-icon1.png" v-if="!item.isSelect" 
						@click="choose(index)"></image></view>
						
					</view>
					<view class="R_cont d-flex j-sb a-center">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="infor">
							<view class="tit avoidOverflow2">{{item.title}}</view>
							<view class="d-flex font-24 color6 mt">
								<view class="specsBtn mr-1">{{item.sku&&item.sku[item.skuIndex]?item.sku[item.skuIndex].title:''}}</view>
							</view>
							<view class=" B-price">
								<view class="d-flex j-end">
									<view class="numBox d-flex mb-1">
										<view class="btn" @click="counter(index,'-')">-</view>
										<view class="num">{{item.count}}</view>
										<view class="btn pubBj white add" @click="counter(index,'+')">+</view>
									</view>
								</view>
								<view class="d-flex  a-center">
									<view class="price font-30 font-weight mr-2">{{item.sku&&item.sku[item.skuIndex]?item.sku[item.skuIndex].price:''}}</view>
									<view class="d-flex a-center">
										<view class="font-24">会员</view>
										<view class="VipPrice"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥{{item.sku&&item.sku[item.skuIndex]?item.sku[item.skuIndex].member_price:''}}</view>
									</view>
								</view>
								
							</view>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		
		<!-- 底部结算 -->
		<view class="xqbotomBar d-flex j-sb a-center border-bottom pl-3"  style="height: 100rpx;bottom: 110rpx;z-index: 40;">
			<view class="left d-flex font-26">
				<view class="">
					<image class="seltIcon mr-1" @click="chooseAll" v-show="isChooseAll" src="../../static/images/shopping-icon.png" ></image>
					<image class="seltIcon mr-1" @click="chooseAll" v-show="!isChooseAll" src="../../static/images/shopping-icon1.png" ></image>
				</view>
				<view>全选</view>
			</view>
			<view class="d-flex j-end a-center" v-show="!is_allDelt">
				<view class="font-24 mr-3 d-flex a-center">合计<view class="price font-32 font-weight">{{totalPrice}}</view></view>
				<view class="payBtn font-30 text-white main-bg-color" @click="pay">立即购买</view>
			</view>
			<view class="alldeltBtn border-primary main-text-color main-border-color text-center border rounded50 mr-3" @click="deleteAll()" v-show="is_allDelt">删除</view>
		</view>
		
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/classify/classify'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">分类</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/VIP/VIP'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">VIP</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar4-a.png" />
				</view>
				<view class="text this-text">购物车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar5.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
				count:1,
				mainData:[
					
				],
				is_allDelt:false,
				totalPrice:0,
				isChooseAll:true
			}
		},
		
		onLoad() {
			const self = this;
			
		},
		
		onShow() {
			const self = this;
			self.mainData = self.$Utils.getStorageArray('cartData');
			console.log('self.mainData',self.mainData)
			self.checkChooseAll();
			self.$Utils.loadAll(['getUserInfoData'], self);
			
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
						self.countTotalPrice();
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			pay(e) {
				const self = this;
				const orderList = [
				];
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						orderList.push(
							{sku_id:self.mainData[i].sku[self.mainData[i].skuIndex].id,count:self.mainData[i].count,
							product:self.mainData[i],skuIndex:self.mainData[i].skuIndex},
						);
					};
				};
				if (orderList.length == 0) {
					self.$Utils.showToast('未选择商品', 'none', 1000);
					return;
				};
				uni.setStorageSync('payPro', orderList);
				self.$Router.navigateTo({
					route: {
						path: '/pages/orderConfim/orderConfim'
					}
				})
			},
			
			checkChooseAll() {
				const self = this;
				var isChooseAll = true;
				for (var i = 0; i < self.mainData.length; i++) {
					if (!self.mainData[i].isSelect) {
						isChooseAll = false;
					};
				};
				self.isChooseAll = isChooseAll;
			},
			
			chooseAll() {
				const self = this;
				self.isChooseAll = !self.isChooseAll;
				for (var i = 0; i < self.mainData.length; i++) {
					self.mainData[i].isSelect = self.isChooseAll;
					self.$Utils.setStorageArray('cartData', self.mainData[i], 'id', 999);
				};
				self.countTotalPrice();
			},
			
			allDeltShow(){
				const self = this;
				self.is_allDelt = !self.is_allDelt
			},
			
			counter(index,type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				self.countTotalPrice();
			},
			
			deleteAll() {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认要删除选中商品吗？',
					showCancel: true,
					cancelText: '取消',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							for (var i = 0; i < self.mainData.length; i++) {
								if(self.mainData[i].isSelect){
									self.$Utils.delStorageArray('cartData', self.mainData[i], 'id');
								}
							};
							self.mainData = self.$Utils.getStorageArray('cartData');
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					},
				});
			},
			
			
			
			choose(index) {
				const self = this;
				
				if (self.mainData[index].isSelect) {
					self.mainData[index].isSelect = false;
				} else {
					self.mainData[index].isSelect = true;
				};
				self.$Utils.setStorageArray('cartData', self.mainData[index], 'id', 999);
				
				self.checkChooseAll();
				self.countTotalPrice();
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				var nowTime = (new Date()).getTime() / 1000;
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						if(self.userInfoData.member_time>nowTime){
							self.totalPrice += self.mainData[i].sku[self.mainData[i].skuIndex].member_price * self.mainData[i].count;
						}else{
							self.totalPrice += self.mainData[i].sku[self.mainData[i].skuIndex].price * self.mainData[i].count;
						}
					};
				};
				console.log(self.totalPrice)
			},
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proRow.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 240rpx;background: #F5F5F5;}
	
	.proRow .item .pic{width: 180rpx;height: 180rpx;}
	.proRow .item .infor{ width:66%;height: 180rpx;}
	.proRow .item{margin-bottom: 30rpx; align-items: center;padding: 30rpx 20rpx;}
	.proRow .item .R_cont{width: 92%; align-items: flex-start;}
	
	/* 商城商品列表 */
	.item_selBtn{width: 8%;}
	.seltIcon{width:36rpx; height: 36rpx; display: block;}
	.shopIcon{width: 30rpx; height: 30rpx; display: block;}
	.alldeltBtn{width: 150rpx;height: 54rpx;line-height: 50rpx;}
	
</style>
