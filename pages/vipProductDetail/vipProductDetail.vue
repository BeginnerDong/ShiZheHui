<template>
	<view>
		
		<view class="detailBanner">
			<swiper class="swiper-box rounded10 overflow-h" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-color="#adadad" indicator-active-color="#ffbcd7">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="pl-3 py-2 main-bg-color d-flex a-center j-sb detail-priceBox text-white" style="height: 100rpx;">
			<view style="width: 85%;">
				<view class="d-flex a-center">
					<view class="price font-30 font-weight mr-2">{{mainData.price?mainData.price:''}}</view>
				</view>
				
			</view>
			<view class="shareBtn d-flex j-center a-center font-24"><image class="shareIcon" src="../../static/images/details-icon3.png" mode=""></image><view class="ml-1">分享</view></view>
		</view>
		<view class="mx-3 py-3">
			<view class="font-32 font-weight pb-2">{{mainData.title}}</view>
			<view class="font-26 color9">{{mainData.description}}</view>
		</view>
		
		
		<view class="f5Bj-H20"></view>
		<view class="px-3">
			<view class="tooling_indNav color6 my-3 mx-4">
				<view class="list rounded50 d-flex text-center j-sb f5bj ">
					<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">详情介绍</view>
					<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">商品评论</view>
				</view>
			</view>
			<view class="py-3">
				<view class=" xqInfor" v-show="curr==1">
					<view class="cont fs14 text-center">
						<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
						</view>
					</view>
				</view>
				<!-- 评论 -->
				<view v-show="curr==2">
					<view class="detail_pjLis" v-if="messageData.length>0">
						<view class="item" v-for="(item,index) in messageData" :key="index">
							<view class="d-flex j-sb a-center pb-1 font-24">
								<view class="d-flex a-center">
									<view class="photo mr-1"><image :src="item.mainImg&&item.mainImg[0]&&item.mainImg[0].url!=''?item.mainImg[0].url:''" mode=""></image></view>
									<view class="name color6">{{item.title!=''?item.title:'用户'}}</view>		
								</view>
								<view class="time color9">{{item.create_time}}</view>
							</view>
							<view class="text font-26 pt-1">{{item.description}}</view>
							<view class="picLis d-flex a-start flex-wrap" >
								<view class="pic" v-for="(c_item,c_index) in item.bannerImg">
									<image :src="item.url" mode=""></image>
								</view>
							</view>
						</view>
					</view>
					<view v-else style="width: 100%;text-align: center;">暂无评论</view>
				</view>
			</view>
			
		</view>
		
		<view class="xqbotomBar center pl-3">
			<view class="d-flex fs12">
				<view class="ite flexColumn" @click="navigateBack">
					<view class="icon"><image src="../../static/images/details-icon4.png" mode=""></image></view>
					<view class="mt-1">返回</view>
				</view>
			</view>
			<view class=" d-flex text-white font-30 text-center">
				<view class="payBtn main-bg-color" style="width: 560rpx;" @click="goBuy">立即购买</view>
			</view>
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
				labelData: [
					"../../static/images/details-img.png",
					"../../static/images/details-img.png",
					"../../static/images/details-img.png"
				],
				specsCurr:0,
				is_spaceShow:false,
				seltSpecsData:['水动力三部曲套装','水动力三部曲套装','定制版','定制版100ML','定制版120ML'],
				curr:1,
				
				mainData:{},
				messageData:[],
				orderList:[],
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			if(options.type){
				self.type = options.type
			};
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;		
				const postData = {};
				var nowTime = (new Date()).getTime() / 1000;
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					order:{
						tableName:'Order',
						middleKey:'user_no',
						key:'user_no',
						searchItem:{
							type:6,
							pay_status:1,
							
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					}
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			currChange(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},

			
			getMainData() {
				const self = this;
				var now = new Date().getTime();
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					self.getMessageData();
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
			
			getMessageData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 3
				};
				postData.searchItem = {
					thirdapp_id: 2,
					product_no: self.mainData.product_no,
					type: 1,
					user_type: 0
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData.push.apply(self.messageData, res.info.data);
					}
					console.log('self.messageData', self.messageData)
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			goBuy() {
				const self = this;
				
				if(self.userInfoData.member==0){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('您还不是会员身份，无权限领取', 'none');
					return
				};
				if(self.type&&self.type=='new'&&self.userInfoData.order.length>0){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('您已领取过新会员福利', 'none');
					return
				};
				if(!self.type&&self.userInfoData.member!=0&&self.userInfoData.member!=self.mainData.member){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('会员身份不符', 'none');
					return
				};
				if(!self.type&&self.userInfoData.member!=0&&self.userInfoData.member==self.mainData.member&&self.userInfoData.behavior==1){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('您已领取过该福利', 'none');
					return
				};
				if(self.mainData.stock==0){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('库存不足！', 'none');
					return
				};
				uni.setStorageSync('canClick', false);
				self.orderList.push(
					{product_id:self.mainData.id,count:1,
					type:1,product:self.mainData},
				);
				uni.setStorageSync('payPro', self.orderList);
				self.Router.navigateTo({
					route: {
						path: '/pages/vipOrderConfim/vipOrderConfim'
					}
				})
				uni.setStorageSync('canClick', true);
			},
			

		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";
	@import "../../assets/style/productList.css";
	page{padding-bottom:140rpx;}
	
	/* .banner-box {width: 100%;height: 420rpx;box-sizing: border-box;overflow: hidden;} */
	.swiper-box {height: 420rpx;box-sizing: border-box;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	
	.bottom-btnCont{width: 520rpx;line-height: 80rpx;}
	.bottom-btnCont .hei{background-color: #3c3c3c;}
	
	.specsLable{flex-wrap: wrap;}
	.specsLable .tt{margin: 30rpx 50rpx 0 0;border: 1px solid #ddd;line-height: 60rpx;padding: 0 16rpx;border-radius:10rpx;}
	
	.specsLable .tt.on{background-color: #fff2f5;color: #ffb2c2;border: 1px solid #ffb2c2;}
	
	.spaceShow{width: 100%;border-radius: 20rpx 20rpx 0 0;position: fixed;left: 0;right: 0;bottom: 0;height: 800rpx;z-index: 50;padding: 30rpx;padding-bottom: 140rpx;}
	.spaceShow .pic{width: 210rpx;height: 210rpx;}
	
	.main-bgTwo{background-color: #ffcdd7;}
</style>
