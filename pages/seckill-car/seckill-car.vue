<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="mx-3">
			<view class="py-3 d-flex j-sb a-center">
				<view>全部宝贝(3)</view>
				<view class="fs13"  v-show="!is_allDelt" @click="allDeltShow">管理</view>
				<view class="fs13"  v-show="is_allDelt" @click="allDeltShow">完成</view>
			</view>
			
			<view class="proRow">
			 	<view class="item d-flex j-sb a-center  position-relative" v-for="(item,index) in mainData" :key="index">
					<view class="item_selBtn flex">
						<view><image class="seltIcon" src="../../static/images/shopping-icon.png" v-if="item.isSelect" @click="choose(index)"></image>
						<image class="seltIcon" src="../../static/images/shopping-icon1.png" v-if="!item.isSelect" @click="choose(index)"></image></view>
						
					</view>
					<view class="R_cont d-flex j-sb a-center">
						<view class="pic"><image src="../../static/images/shopping-icon4.png" mode=""></image></view>
						<view class="stateText font-24 text-white" style="background-color: #BDBDBD;">已失效</view>
						<view class="stateText font-24 text-white">进行中</view>
						<view class="infor">
							<view class="tit avoidOverflow">墨西哥牛油果8枚单果200g左右</view>
							<view class="d-flex font-24 color6 mt">
								<view class="specsBtn mr-1">精装品5斤</view>
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
									<view class="price font-30 font-weight mr-2">{{item.price}}</view>
									<view class="d-flex a-center">
										<view class="font-24">会员</view>
										<view class="VipPrice"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view>
									</view>
								</view>
								
							</view>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		
		<!-- 底部结算 -->
		<view class="xqbotomBar d-flex j-sb a-center border-bottom pl-3"  style="height: 100rpx;bottom: 00rpx;z-index: 40;">
			<view class="left d-flex font-26">
				<view class="">
					<image class="seltIcon mr-1" @click="chooseAll" v-show="isChooseAll" src="../../static/images/shopping-icon.png" ></image>
					<image class="seltIcon mr-1" @click="chooseAll" v-show="!isChooseAll" src="../../static/images/shopping-icon1.png" ></image>
				</view>
				<view>全选</view>
			</view>
			<view class="d-flex j-end a-center" v-show="!is_allDelt">
				<view class="font-24 mr-3 d-flex a-center">合计<view class="price font-32 font-weight">{{totalPrice}}</view></view>
				<view class="payBtn font-30 text-white main-bg-color"  @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">立即购买</view>
			</view>
			<view class="alldeltBtn border-primary main-text-color main-border-color text-center border rounded50 mr-3" @click="deleteAll()" v-show="is_allDelt">删除</view>
		</view>
		
		
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
					{isSelect:true,price:'88',count:'1'},
					{isSelect:false,price:'88',count:'1'}
				],
				is_allDelt:false,
				isChooseAll:false,
				totalPrice:"88"
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
				
				for (var i = 0; i < self.mainData.length; i++) {
					if (self.mainData[i].isSelect) {
						self.totalPrice += self.mainData[i].price * self.mainData[i].count;
					};
				};
				console.log(self.totalPrice)
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 240rpx;background: #F5F5F5;}
	
	.proRow .item .pic{width: 180rpx;height: 180rpx;}
	.proRow .item .infor{ width:66%;height: 180rpx;}
	.proRow .item .infor .tit{ width:80%;}
	.proRow .item{margin-bottom: 30rpx; align-items: center;padding: 30rpx 20rpx;}
	.proRow .item .R_cont{width: 92%; align-items: flex-start;}
	
	/* 商城商品列表 */
	.item_selBtn{width: 8%;}
	.seltIcon{width:36rpx; height: 36rpx; display: block;}
	.shopIcon{width: 30rpx; height: 30rpx; display: block;}
	.alldeltBtn{width: 150rpx;height: 54rpx;line-height: 50rpx;}
	
	
	.stateText{width: 100rpx;height: 40rpx;line-height: 40rpx;background-color: #40c77f;position: absolute;right: 0;top: 16rpx;text-align: center;border-radius: 30rpx 0 0 30rpx;}
</style>
