<template>
	<view>
		
		<view class="bg-white boxShaow overflow-h mt-3 mb-3">
			
			<view class="px-3 py-3">
				<view class="tabBtn d-flex a-center color6 font-26">
					<view class="tt pb-2 mr-5" :class="curr==1?'on color2 font-weight position-relative':''" @click="currChange(1)">快递包邮</view>
					<view class="tt pb-2" :class="curr==2?'on color2 font-weight position-relative':''" @click="currChange(2)">到店自提</view>
				</view>
				<view v-show="curr==1">
					
					<view class="d-flex j-sb a-center" v-if="addressData.name"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="font-28">
							<view class="d-flex mt-1">
								<view class="mr-3 color2">{{addressData.name}}</view>
								<view class="color6">{{addressData.phone}}</view>
							</view>
							<view class="mt-2">{{addressData.city+addressData.detail}}</view>
						</view>
						<view class="d-flex j-end" style="width: 20%;">
							<image class="arrowR" src="../../static/images/detailsl-icon1.png" mode=""></image>
						</view>
					</view>
					
					<view class="d-flex j-center pt-3 pb-4" v-else @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="font-28 color6 d-flex">
							<view style="width: 40rpx;height:40rpx;margin-right: 10rpx;"><image src="../../static/images/indianal-icon1.png" mode=""></image></view>
							<view class="font-30">选择收货地址</view>
						</view>
					</view>
				</view>
				
				<view class="pt-3" v-show="curr==2">
					<view class="d-flex j-sb a-center" v-if="shopData.name" 
					@click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
						<view style="width: 85%;">
							<view class="d-flex a-center">
								<view class="mr-3 font-30">{{shopData.name}}</view>
								<view class="font-26">{{shopData.phone}}</view>
							</view>
							<view class="mt-1">{{shopData.address}}</view>
						</view>
						<view class="arrowR"><image src="../../static/images/home-icon7.png" mode=""></image></view>
					</view>
					<view class="d-flex j-sb a-center" v-else
					@click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
						<view style="width: 85%;">
							<view class="mt-1">选择自提地址</view>
						</view>
						<view class="arrowR"><image src="../../static/images/home-icon7.png" mode=""></image></view>
					</view>
					<view class="d-flex a-center j-sb pt-3">
						<view class="editMsg rounded10"><input type="text" v-model="orderInfo.name" placeholder="输入姓名" placeholder-class="placeholder" /></view>
						<view class="editMsg rounded10"><input type="number" maxlength="11" v-model="orderInfo.phone" placeholder="输入电话" placeholder-class="placeholder" /></view>
					</view>
				</view>
			</view>
		</view>
		<view class="proRow mt-3">
			<view class="bg-white  mb-3" v-for="(item,index) in mainData" :key="index">
				<view class="item d-flex j-sb">
					<view class="pic"><image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image></view>
					<view class="infor position-relative">
						<view class="tit avoidOverflow">{{item.product?item.product.title:''}}</view>
						<view class="d-flex font-24 color6 mt">
							<view class="specsBtn mr-1">{{item.product&&item.product.sku&&item.product.sku[item.skuIndex]?item.product.sku[item.skuIndex].title:''}}</view>
						</view>
						<view class="B-price position-absolute bottom-0 left-0">
							<view class="d-flex  a-center">
								<view class="price font-30 font-weight mr-2">{{item.product&&item.product.sku&&item.product.sku[item.skuIndex]?item.product.sku[item.skuIndex].price:''}}</view>
								<view class="d-flex a-center">
									<view class="font-24">会员</view>
									<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥{{item.product&&item.product.sku&&item.product.sku[item.skuIndex]?item.product.sku[item.skuIndex].member_price:''}}</view>
								</view>
							</view>
						</view>
					</view>
				</view>
				
				<view class="d-flex j-end mr-3 pb-3">
					<view class="numBox d-flex">
						<view class="btn" @click="counter(index,'-')">-</view>
						<view class="num">{{item.count}}</view>
						<view class="btn pubBj white add" @click="counter(index,'+')">+</view>
					</view>
				</view>
			</view>
		</view>
		<view class="px-3 py-3 pdlr4 d-flex whiteBj pdt15 pdb15 radius8 mgt15">
			<view>留言：</view>
			<view class="BZ-Text"><input type="text" v-model="passage1" placeholder="请输入留言信息" placeholder-class="" /></view>
		</view>
		<view class="xqbotomBar pl-3">
			<view class="d-flex a-center mr-3 font-26">总计<view class="price font-weight font-30">{{totalPrice}}</view></view>
			<button style="border-radius: 0;" class="payBtn main-bg-color font-30 text-white" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">立即支付</button>
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				count:1,
				
				shopData:{},
				addressData:{},
				pay:{},
				totalPrice:0,
				mainData:[],
				nearShopData:[],
				orderInfo:{
					name:'',
					phone:''
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			console.log('self.mainData',self.mainData)
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		onShow() {
			const self = this;
			if(self.curr==2){
				if(uni.getStorageSync('choosedShopData')){
					self.shopData = uni.getStorageSync('choosedShopData')
				}else{
					self.getDefalutShop()
				};
			}else{
				if(uni.getStorageSync('choosedAddressData')){
					self.addressData = uni.getStorageSync('choosedAddressData')
				}else{
					self.getAddressData()
				};
			}
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
			
			//获取默认门店
			getDefalutShop() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.getAfter = {
					shop:{
						tableName:'UserInfo',
						middleKey:'passage1',
						key:'user_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				}
				const callback = (res) => {
					if (res.info.data.length > 0) {
						if(res.info.data[0].shop[0]){
							self.shopData= res.info.data[0].shop[0]
						}
					}
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			
			//获取默认收货地址
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			currChange(curr){
				const self = this;	
				if(curr!=self.curr){
					self.curr = curr
					if(self.curr==2){
						if(uni.getStorageSync('choosedShopData')){
							self.shopData = uni.getStorageSync('choosedShopData')
						}else{
							self.getDefalutShop()
						};
					}else{
						if(uni.getStorageSync('choosedAddressData')){
							self.addressData = uni.getStorageSync('choosedAddressData')
						}else{
							self.getAddressData()
						};
					};
					self.countTotalPrice()
				}
			},
			
			
			countTotalPrice() {
				const self = this;
				var nowTime = (new Date()).getTime() / 1000;
				self.totalPrice = 0;
				self.oldPrice = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					if(self.userInfoData.member_time>nowTime){
						self.totalPrice += self.mainData[i].product.sku[self.mainData[i].skuIndex].member_price*self.mainData[i].count
					}else{
						self.totalPrice += self.mainData[i].product.sku[self.mainData[i].skuIndex].price*self.mainData[i].count
						
					}
				};
				
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
				if(self.userInfoData.member_time>nowTime){
					for (var i = 0; i < self.mainData.length; i++) {
						self.oldPrice += self.mainData[i].product.sku[self.mainData[i].skuIndex].price*self.mainData[i].count
					};
					
					self.save = parseFloat(parseFloat(self.oldPrice)-self.totalPrice).toFixed(2)
				};
				
				console.log('self.save',self.save)
				if (self.totalPrice > 0) {
					self.pay.wxPay = {
						price: self.totalPrice,
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				console.log(self.pay)
			},
			
			counter(index, type) {
				const self = this;
				if (type == '+') {
					self.mainData[index].count++;
				} else {
					if (self.mainData[index].count > 1) {
						self.mainData[index].count--;
					}
				};
				self.countTotalPrice();
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.curr==2){
					if(JSON.stringify(self.shopData) == '{}'){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请选择自提门店','none')
						return
					};
					if(self.orderInfo.name==''||self.orderInfo.phone==''){
						
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请填写收货人信息','none')
						return
					}else{
						if(self.orderInfo.phone!=''){
							if (self.orderInfo.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.orderInfo.phone)) {
								uni.setStorageSync('canClick', true);
								self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
								return;
							}
						};
					}
				}else{
					if(JSON.stringify(self.addressData) == '{}'){
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('请选择收货地址','none')
						return
					}
				};
				var data = {
					transport_type:self.curr,
					snap_address:self.addressData,
				};
				if(self.curr==2){
					data.name = self.orderInfo.name;
					data.phone = self.orderInfo.phone;
					data.shop_no = self.shopData.user_no
				};
				var orderList = []
				for (var i = 0; i < self.mainData.length; i++) {
					orderList.push({sku_id:self.mainData[i].sku_id,count:self.mainData[i].count,type:self.mainData[i].product.type,data: data,
					snap_address: self.addressData})
				}
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {
					transport_type:self.curr,
					level:1,
					snap_address:self.addressData,
					//price:self.totalPrice
					type:1
				};
				postData.type = 1;
				postData.parent = 1;
				postData.tokenFuncName = 'getProjectToken';
				if(self.curr==2){
					postData.data.name = self.orderInfo.name;
					postData.data.phone = self.orderInfo.phone;
					postData.data.shop_no = self.shopData.user_no;
				};
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						var array = self.$Utils.getStorageArray('cartData');
						for (var i = 0; i < orderList.length; i++) {
							for (var j = 0; j < array.length; j++) {
								if(orderList[i].sku_id == array[j].sku[array[j].skuIndex].id){
									self.$Utils.delStorageArray('cartData', array[j], 'id');
								}
							}
						};
						
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay() {
				const self = this;	
				const postData = self.$Utils.cloneForm(self.pay);	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				if(self.save){
					postData.payAfter = [
						{
							tableName: 'UserInfo',
							FuncName: 'update',
							searchItem:{
								user_no:uni.getStorageSync('user_info').user_no
							},
							data: {
								save:parseFloat(parseFloat(self.userInfoData.save)+parseFloat(self.save)).toFixed(2)
							},
						},
					];
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
					
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
									}, 1000);
									
									
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
							}, 1000);
							
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;box-sizing: border-box;}
	
	/* .tabBtn .tt.on{color2: #222;font-weight: bold;} */
	.tabBtn .tt.on::before{content: "";width: 0;height: 0;border-right:14rpx solid transparent;border-left:14rpx solid transparent;border-top:14rpx solid #ffb2c2;position: absolute;left: 50%;bottom: 0;transform: translateX(-50%);}
	
	.proRow .item .pic{width: 180rpx;height: 180rpx;}
	.proRow .item .infor{width: 70%;height: 180rpx;}
	
	.editMsg{width: 48%;height: 60rpx;padding: 0 10rpx;box-sizing: border-box;border: 1px solid #eee;box-sizing: border-box;text-align: center;}
	.editMsg input{width: 100%;height: 60rpx;line-height: 60rpx;box-sizing: border-box;text-align: center;font-size: 26rpx;}
	
	/* .specsBtn{padding: 0 10rpx;line-height: 44rpx;background-color: #f7f7f7;border-radius: 6rpx;} */
</style>
