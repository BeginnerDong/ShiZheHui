<template>
	<view>
		
		<view class="vipHead">
			<view class="position-relative left-0 top-0 right-0" style="height: 270rpx;"><image src="../../static/images/vip-icon0.png" mode="widthFix"></image></view>
		</view>
		
		<view class="mx-3">
			
			<!-- 会员卡 -->
			<view class="vipCard rounded10 overflow-h p-3 position-relative">
				<view class="position-absoluteXY"><image src="../../static/images/vip-icon01.png" mode=""></image></view>
				<view class="cont position-relative main-text-color">
					<view style="height: 176rpx;">
						<view class="font-36 font-weight">会员卡/VIP</view>
						<view class="mt-1 font-26">{{isMember&&isMember==true?'按您的消费计算，已为您节省￥'+save+'元':'按一年用户消费计算，每年会节省￥'+yearSave+'元'}}</view>
					</view>
					
					<view class="d-flex a-center j-sb">
						<view class="font-26" style="width: 65%;color: #f27f97;" v-if="!isMember">美蘑街，专业会员制电商平台</view>
						<view class="font-26" style="width: 65%;color: #f27f97;" v-else>有效期至：{{Utils.timeto(userInfoData.member_time*1000,'ymd')}}</view>
						<view class="btn text-white text-center font-28" v-if="!isMember" @click="goBuy">{{isMember&&isMember==true?'立即续费':'立即开通'}}</view>
						<!-- <view class="btn text-white text-center font-28" v-if="!isMember" @click="Router.navigateTo({route:{path:'/pages/vipRealname/vipRealname'}})">{{isMember&&isMember==true?'立即续费':'立即开通'}}</view> -->
					</view>
				</view>
			</view>
			
			<!-- 四大权益 -->
			<view class="VipFour mt-2 bg-white rounded10 py-3 px-4 main-text-color text-center">
				<view class="font-32 font-weight mb-3">会员专享4大权益</view>
				<view class=" d-flex a-center j-sb "  @click="vipQyBoxShow">
					<view class="item">
						<view class="icon"><image src="../../static/images/vip-icon2.png" mode=""></image></view>
						<view class="font-26 mt-2">优惠独享</view>
						<view class="text2 font-24 mt">专享秒杀</view>
					</view>
					<view class="item">
						<view class="icon"><image src="../../static/images/vip-icon3.png" mode=""></image></view>
						<view class="font-26 mt-2">快人一步</view>
						<view class="text2 font-24 mt">优先发货</view>
					</view>
					<view class="item">
						<view class="icon"><image src="../../static/images/vip-icon4.png" mode=""></image></view>
						<view class="font-26 mt-2">会员独享</view>
						<view class="text2 font-24 mt">会员礼</view>
					</view>
					<view class="item">
						<view class="icon"><image src="../../static/images/vip-icon5.png" mode=""></image></view>
						<view class="font-26 mt-2">一网打尽</view>
						<view class="text2 font-24 mt">吃喝玩乐</view>
					</view>
				</view>
			</view>
		
			<!-- 首冲会员享受优惠 -->
			<view class="pinTab oh px-2 mt-2 py-3 bg-white rounded10">
				<view class="font-30 font-weight d-flex j-sb a-center">首充会员享受优惠</view>
				<view class="d-flex pt-3 itemBox">
					<view class="item" v-for="(item,index) in mainData" v-if="item.member==0" :key="index" :data-id="item.id"
					 @click="Router.navigateTo({route:{path:'/pages/vipProductDetail/vipProductDetail?id='+$event.currentTarget.dataset.id+'&type=new'}})">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="tit font-24 avoidOverflow">{{item.title}}</view>
						<view class="yuanJia mb-1">{{item.o_price}}</view>
						<view class="d-flex a-center font-20 red"><span class="price font-weight font-28">{{item.price}}</span></view>
					</view>
				</view>
			</view>
			
			<!-- 月卡礼品 -->
			<view class="pinTab oh px-2 mt-2 py-3 bg-white rounded10">
				<view class="font-30 font-weight d-flex j-sb a-center">月卡礼品</view>
				<view class="d-flex pt-3 itemBox">
					<view class="item" v-for="(item,index) in mainData" v-if="item.member==1" :key="index" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/vipProductDetail/vipProductDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="tit font-24 avoidOverflow">{{item.title}}</view>
						<view class="yuanJia mb-1">{{item.o_price}}</view>
						<view class="d-flex a-center font-20 red"><span class="price font-weight font-28">{{item.price}}</span></view>
					</view>
				</view>
			</view>
			
			<!-- 季卡礼品 -->
			<view class="pinTab oh px-2 mt-2 py-3 bg-white rounded10">
				<view class="font-30 font-weight d-flex j-sb a-center">季卡礼品</view>
				<view class="d-flex pt-3 itemBox">
					<view class="item" v-for="(item,index) in mainData" v-if="item.member==2" :key="index" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/vipProductDetail/vipProductDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="tit font-24 avoidOverflow">{{item.title}}</view>
						<view class="yuanJia mb-1">{{item.o_price}}</view>
						<view class="d-flex a-center font-20 red"><span class="price font-weight font-28">{{item.price}}</span></view>
					</view>
				</view>
			</view>
			
			<!-- 半年卡礼品 -->
			<view class="pinTab oh px-2 mt-2 py-3 bg-white rounded10">
				<view class="font-30 font-weight d-flex j-sb a-center">半年卡礼品</view>
				<view class="d-flex pt-3 itemBox">
					<view class="item" v-for="(item,index) in mainData" v-if="item.member==3" :key="index" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/vipProductDetail/vipProductDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="tit font-24 avoidOverflow">{{item.title}}</view>
						<view class="yuanJia mb-1">{{item.o_price}}</view>
						<view class="d-flex a-center font-20 red"><span class="price font-weight font-28">{{item.price}}</span></view>
					</view>
				</view>
			</view>
			
			<!-- 年卡礼品 -->
			<view class="pinTab oh px-2 mt-2 py-3 bg-white rounded10">
				<view class="font-30 font-weight d-flex j-sb a-center">年卡礼品</view>
				<view class="d-flex pt-3 itemBox">
					<view class="item" v-for="(item,index) in mainData" v-if="item.member==4" :key="index" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/vipProductDetail/vipProductDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="tit font-24 avoidOverflow">{{item.title}}</view>
						<view class="yuanJia mb-1">{{item.o_price}}</view>
						<view class="d-flex a-center font-20 red"><span class="price font-weight font-28">{{item.price}}</span></view>
					</view>
				</view>
			</view>
		
		</view>
		
		<!-- 选择会员卡种类 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="VipBuy bg-white border-bottom" v-show="is_VipBuy">
			<view class="closebtn" @click="VipBuyShow">×</view>
			<view class="font-32 font-weight text-center pt pb-4 border-bottom">选择会员卡种类</view>
			<view class="d-flex a-center j-sb py-4 border-bottom font-weight">
				<view class="seltCard d-flex j-center a-center" :class="vipCurr==index?'on':''" 
				v-for="(item,index) in cardData" :key="index" @click="vipChange(index)">
					<view class="flexColumn">
						<view class="d-flex j-center font-26 mb-1">
							<image class="icon" :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
							<view>{{item.title}}</view>
						</view>
						<view class="font-24 d-flex a-center j-center">￥<span class="font-40">{{item.price}}</span></view>
					</view>
					<view class="seltIcon"><image :src="vipCurr==index?'../../static/images/vip-icon11.png':''" mode=""></image></view>
				</view>
			</view>
			<view class="py-4 border-bottom d-flex a-center j-sb">
				<view class="font-30 font-weight">支付方式</view>
				<view class="d-flex j-end a-center font-26">
					<image style="width: 38rpx;height: 36rpx;margin-right: 6rpx;" src="../../static/images/vip-icon12.png" mode=""></image>
					<view>微信支付</view>
				</view>
			</view>
			<view class="twoBtn position-relative text-white text-center">
				<view class="position-absoluteXY"><image src="../../static/images/vip-icon14.png" mode=""></image></view>
				<view class="cont d-flex a-center j-sb font-weight">
					<view class="btn font-40" style="width: 440rpx;">￥{{cardData[vipCurr]?cardData[vipCurr].price:''}}</view>
					<button class="btn font-30" style="width: 250rpx;background-color: none;" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确认支付</button>
				</view>
			</view>
		</view>
		
		<!-- 四大权限弹框 -->
		<view class="vipQyBox rounded10" v-show="is_vipQy">
			<view class="closeBtn" @click="vipQyBoxShow">×</view>
			<swiper class="swiper" :previous-margin="swiper.margin" :next-margin='swiper.margin' :circular="true" @change="swiperChange">
				<swiper-item v-for="(item,index) in swiper.list" :key="index" >
					<view class="list" :class="{'le-active':swiper.index == index}">
						<view class="hei d-flex a-center main-text-color p-3 rounded10">
							<view class="logo mr-2"><image src="../../static/images/vip-icon2.png" mode=""></image></view>
							<view>
								<view class="font-weight">{{item.title}}</view>
								<view class="fs20">{{item.description}}</view>
							</view>
						</view>
						
						<view class="xqInfor font-26 mt-4">
							<view class="content ql-editor" style="padding:0;" v-html="item.content">
							</view>
						</view>
					</view>
				</swiper-item>
			</swiper>
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
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">VIP</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar4.png" />
				</view>
				<view class="text">购物车</view>
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
				
				is_VipBuy:false,
				vipCurr:0,
				
				is_vipQy:false,
				
				swiper: {
					margin: "105rpx",
					index: 0,
					list: []
				},
				searchItem:{
					thirdapp_id:2,
					type:6
				},
				mainData:[],
				artData:[],
				cardData:[],
				Utils:this.$Utils,
				isMember:'',
				save:'',
				userInfoData:{},
				yearSave:0
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.showLoading();
			const callback = (res) => {
				self.yearSave = uni.getStorageSync('user_info').thirdApp.save;
				self.$Utils.loadAll(['getUserInfoData','getMainData','getArtData','getCardData'], self);
			};
			self.$Token.getProjectToken(callback, {
				refreshToken: true
			})
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('infoOk')){
				uni.removeStorageSync('infoOk')
				self.getUserInfoData()
				self.VipBuyShow()
			}
		},
		
		methods: {
			
			goBuy(){
				const self = this;
				if(self.userInfoData.name!=''){
					self.VipBuyShow()
				}else{
					self.Router.navigateTo({route:{path:'/pages/vipRealname/vipRealname'}})
				}
			},
			
			getUserInfoData() {
				const self = this;		
				const postData = {};
				var nowTime = (new Date()).getTime() / 1000;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						if(self.userInfoData.member_time>nowTime){
							self.isMember = true;
							self.save = self.userInfoData.save
						}else{
							self.isMember = false;
							self.save = uni.getStorageSync('user_info').thirdApp.save
						}
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick', false);
				var orderList = []
				orderList.push({product_id:self.cardData[self.vipCurr].id,count:1})
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {
					type:5
					//level:1,
					//price:self.totalPrice
				};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
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
				const postData = {};
				postData.wxPay = {
					price:parseFloat(self.cardData[self.vipCurr].price).toFixed(2),
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};
				postData.payAfter = [
					{
						tableName: 'UserInfo',
						FuncName: 'update',
						searchItem:{
							user_no:uni.getStorageSync('user_info').user_no
						},
						data: {
							behavior:0,
							member:self.vipCurr+1,
							member_time:(self.userInfoData.member_time==0?(new Date()).getTime() / 1000:self.userInfoData.member_time)+self.cardData[self.vipCurr].duration*86400
						},
					},
				];
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									self.$Utils.showToast('开通成功！','none')
									self.is_show = !self.is_show;
									self.is_VipBuy = !self.is_VipBuy;
									self.getUserInfoData()
								} else {
									uni.setStorageSync('canClick', true);
									self.$Utils.showToast(res.msg,'none')
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							self.$Utils.showToast('开通成功！','none')
							self.is_show = !self.is_show;
							self.is_VipBuy = !self.is_VipBuy
							self.getUserInfoData()
						};
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(res.msg,'none')
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			vipChange(index){
				const self = this;
				self.vipCurr = index
			},
			
			VipBuyShow(){
				const self = this;
					self.is_show = !self.is_show;
					self.is_VipBuy = !self.is_VipBuy
			},
			
			vipQyBoxShow(){
				const self = this;
					self.is_show = !self.is_show;
					self.is_vipQy = !self.is_vipQy
			},
			//swiper滑动事件
			swiperChange: function(e) {
				this.swiper.index = e.detail.current;
			},
			
			getCardData() {
				var self = this;
				var postData = {};
				postData.searchItem = {
					thirdapp_id:2,
					type:5
				};
				postData.order = {
					member: 'asc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.cardData.push.apply(self.cardData, res.info.data);
					};
					self.$Utils.finishFunc('getCardData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getMainData() {
				var self = this;
				var postData = {};
				//postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					listorder: 'desc'
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getArtData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.order = {
					listorder: 'desc'
				};
				postData.getBefore= {
					child:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							status:['in',1],
							title:['in','会员权益']
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						const regex = new RegExp('<img', 'gi');
						for (var i = 0; i < res.info.data.length; i++) {
							
							res.info.data[i].content = res.info.data[i].content.replace(regex, `<img style="max-width: 100%;"`);
						};
						self.swiper.list = res.info.data
					}
					self.$Utils.finishFunc('getArtData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/productList.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 140rpx;background-color: #F5F5F5;}
	.vipCard{height: 300rpx;margin-top: -190rpx;}
	.vipCard .cont{z-index: 2; color: #e95071;}
	.vipCard .btn{width: 160rpx;height: 60rpx;line-height: 60rpx;border-radius: 40rpx;background-image: linear-gradient(to right,#ff9aaf,#ff708e);box-shadow: 0 2px 5px rgba(253,73,110,0.3);}
	
	.VipFour{height: 316rpx;}
	.VipFour .item{height: 182rpx;}
	.VipFour .item .icon{width: 90rpx;height: 90rpx;margin: 0 auto;}
	.VipFour .item .text2{color: #ffc3cf;}
	
	.pinTab .itemBox{overflow-x: auto;}
	.pinTab .item{width: 200rpx;height: 330rpx;margin-right: 26rpx;flex-shrink: 0;}
	/* .pinTab .item:nth-of-type(3n){margin-right: 0;} */
	.pinTab .item .pic{height: 200rpx;}
	
	.black-bj,.VipBuy{bottom: 110rpx;}
	.VipBuy{width: 100%;border-radius: 20rpx 20rpx 0 0;position: fixed;left: 0;right: 0;height: 770rpx;z-index: 50;padding: 30rpx;padding-bottom: 140rpx;}
	.seltCard{width: 165rpx;height: 200rpx;border:1px solid #e1e1e1;position: relative;border-radius: 14rpx;overflow: hidden;}
	.seltCard .icon{width: 34rpx;height: 34rpx;margin-right: 6rpx;}
	.seltCard.on{border: 1px solid #FFB2C2;background: #fff3f8;}
	.seltCard .seltIcon{content: '';width: 62rpx;height: 52rpx;position: absolute;right: 0;bottom: 0;}
	
	.twoBtn{height: 100rpx; margin-top: 80rpx;}
	.twoBtn .cont{position: relative;z-index: 2;line-height: 100rpx;}
	
	/* 权益弹框 */
	.vipQyBox{width: 100%;height: 650rpx;position: fixed;left: 50%;top: 50%;transform: translate(-50%,-50%);z-index: 50;}
	.vipQyBox .hei{background-color: #222222;height: 120rpx;}
	.vipQyBox .logo{width: 70rpx;height: 70rpx;}
	.xqInfor view{line-height: 40rpx;margin-bottom: 10rpx;}
	
	
	.swiper {height: 650rpx;margin: 0 20rpx;}
	.swiper .list {width: 100%;height: 100%;display: block;transform: scale(0.85);transition: transform 0.3s ease-in-out 0s;border-radius: 4px;padding: 30rpx;box-sizing: border-box;background-color: #fff;}
	.swiper .list.le-active {transform: scale(1);}
	button{
		background: none;
	}
</style>

