<template>
	<view>
		
		<view class="px-3 py-3 headTop">
			<view class="d-flex j-sb a-center">
				<view class="d-flex a-center text-white">
					<view class="font-30 font-weight mr-1">{{city!=''?city:'无'}}</view>
					<view class="arrowB"><image src="../../static/images/home-icon.png" mode=""></image></view>
				</view>
				<view class="seachbox" style="width: 75%;">
					<view class="d-flex a-center rr" style="width: 100%;" 
					@click="Router.navigateTo({route:{path:'/pages/seachProduct/seachProduct'}})">
						<button class="seachBtn" type="button"><image src="../../static/images/home-icon1.png" mode=""></image></button>
						<view class="input">
							<input type="text" disabled="true" name="" value="" placeholder="搜索商品" placeholder-class="placeholder" />
						</view>
					</view>
				</view>
			</view>
			<view class="banner-box mt-3">
				<view class="banner">
					<swiper class="swiper-box rounded10 overflow-h"  indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-color="#ffffff" indicator-active-color="#222222">
						<block v-for="(item,index) in sliderData.mainImg" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item.url" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<!-- 菜单 -->
		<view class="indHome d-flex pt-3 font-26" style="flex-wrap: wrap;">
			<view class="item" style="margin-bottom: 30rpx;" v-for="(item,index) in labelData" :key="index" :data-index="index"
			  @click="Router.navigateTo({route:{path:'/pages/classify/classify?index='+$event.currentTarget.dataset.index}})">
				<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''"></image>
				<view class="tit">{{item.title}}</view>
			</view>

		</view>
		
		<!-- <view class="mx-3 d-flex a-center zq-lable" @click="showToast">
			<view class="item p-2 position-relative" style="background-image: linear-gradient(to right,#fff0e5,#ffe4d0);">
				<view class="font-30 font-weight" style="color: #fe8934;" @click="Router.navigateTo({route:{path:'/pages/OneYuan/OneYuan'}})">一元夺宝</view>
				<view class="font-20 color6">去看看</view>
				<view class="icon"><image src="../../static/images/home-icon01.png" mode=""></image></view>
			</view>
			<view class="item p-2 position-relative" style="background-image: linear-gradient(to right,#ffe6e4,#ffc4be);" @click="Router.navigateTo({route:{path:'/pages/seckillList/seckillList'}})">
				<view class="font-30 font-weight" style="color: #ff5545;">秒杀专区</view>
				<view class="font-20 color6">去看看</view>
				<view class="icon"><image src="../../static/images/home-icon02.png" mode=""></image></view>
			</view>
			<view class="item p-2 position-relative" style="background-image: linear-gradient(to right,#e7f3ff,#c2d9ff);" @click="Router.navigateTo({route:{path:'/pages/pintuanlList/pintuanlList'}})">
				<view class="font-30 font-weight" style="color: #5a99ff;">拼团专区</view>
				<view class="font-20 color6">去看看</view>
				<view class="icon"><image src="../../static/images/home-icon03.png" mode=""></image></view>
			</view>
		</view> -->
		
		<!-- GO -->
		<view class="d-flex a-center j-center mt-3">
			<view style="width: 706rpx;height: 198rpx;" 
			@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+noticeData.url}})">
			<image :src="noticeData.mainImg&&noticeData.mainImg[0]?noticeData.mainImg[0].url:''" mode=""></image></view>
		</view>
		
		<!-- 秒杀商品 -->
		<!-- <view class="mx-3 pt-4 pb-3">
			<view class="d-flex a-center j-sb">
				<view class="font-30 font-weight">秒杀商品</view>
				<view class="d-flex j-end a-center color9 font-26" @click="Router.navigateTo({route:{path:'/pages/seckillList/seckillList'}})">查看全部<image class="arrowR" src="../../static/images/home-icon7.png" mode=""></image></view>
			</view>
			<view class="proRow">
				<view class="item d-flex a-start j-sb mt-3" v-for="(item,index) in seckillData" :key="index" @click="Router.navigateTo({route:{path:'/pages/seckillDetail/seckillDetail'}})">
					<view class="pic"><image src="../../static/images/home-img01.png" mode=""></image></view>
					<view class="infor">
						<view class="avoidOverflow font-30">12色可水洗油画棒 水溶性旋转涂料板</view>
						<view class="text-secondary font-24 mt-1">秒杀时间：4月20日16点-18点</view>
						<view class="B-price">
							<view class="d-flex a-center">
								<view class="red">秒杀价<span class="price">79</span></view>
								<view class="yuanJia ml-3">121</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view> -->
		
		<view class="px-3">
			<!-- 今日拼团 -->
			<!-- <view class="pinTab oh px-2 pt-3 bg-white rounded10">
				<view class="name d-flex j-sb a-center">
					<view class="font-30 font-weight ">今日拼团</view>
					<view class="d-flex j-end a-center color9 font-26" @click="Router.navigateTo({route:{path:'/pages/pintuanlList/pintuanlList'}})">查看全部<image class="arrowR" src="../../static/images/home-icon7.png" mode=""></image></view>
				</view>
				<view class="d-flex py-3">
					<scroll-view class="scroll-row" scroll-x >
						<view class="scroll-row-item item" v-for="(item,index) in hotData" :key="index" @click="Router.navigateTo({route:{path:'/pages//'}})">
							<view class="pic"><image src="../../static/images/details-img3.png" mode=""></image></view>
							<view class="tit font-24 avoidOverflow">好孩子儿童餐具宝宝学吃饭</view>
							<view class="yuanJia mb-1">121</view>
							<view class="d-flex a-center font-20 red">团购价<span class="price font-weight font-28">88</span></view>
						</view>
					</scroll-view>
				</view>
			</view> -->
			
			<!-- 商品 -->
			<view class="orderNav d-flex a-start pt-3 color6">
				<view class="tt" :class="curr==-1?'on':''" @click="changeCurr(-1)">热销</view>
				<view class="tt" v-for="(item,index) in labelData" :key="index" v-if="index<4"
				 :class="curr==index?'on':''" @click="changeCurr(index)">{{item.title}}</view>
			</view>
			<view class="d-flex j-sb d-flex j-sb flex-wrap HotProduct">
				<view class="item rounded10 mt-3" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="tit font-30 d-flex a-center">
							<view class="avoidOverflow2">{{item.title}}</view>
						</view>
						<view class="d-flex a-center mt-2">
							<view class="price font-30 font-weight mr-2">{{item.sku&&item.sku[0]?item.sku[0].price:''}}</view>
						</view>
						<view class="d-flex a-center mt-2">
							<view class="font-24">会员</view>
							<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>
							￥{{item.sku&&item.sku[0]?item.sku[0].member_price:''}}</view>
						</view>
						
					</view>
				</view>
			</view>
			
			<!-- 无数据 -->
			<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
			
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
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
				labelData: [],
				seckillData:3,
				hotData:6,
				productData:4,
				curr:-1,
				sliderData:{},
				city:'',
				searchItem:{
					thirdapp_id:2,
					type:1
				},
				mainData:[],
				noticeData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.scene){
				var scene = decodeURIComponent(options.scene);
				const callback = (res) => {
					self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
					self.getLocation();
					self.$Utils.loadAll(['getUserData','getLabelData','getSliderData','getMainData','getNoticeData'], self);
				};
				self.$Token.getProjectToken(callback, {
					refreshToken: true,parent_no:scene
				})
			}else{
				self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				self.getLocation();
				self.$Utils.loadAll(['getUserData','getLabelData','getSliderData','getMainData','getNoticeData'], self);
			}
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			uni.showLoading();
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: '美蘑街',
					path: '/pages/index/index', //点击分享的图片进到哪一个页面
					///imageUrl: self.mainData.mainImg[0].url ? self.mainData.mainImg[0].url : '',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			} else {
				return {
					title: '美蘑街',
					path: '/pages/index/index', //点击分享的图片进到哪一个页面
					///imageUrl: self.mainData.mainImg[0].url ? self.mainData.mainImg[0].url : '',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}
		},
		
		methods: {
			
			showToast(){
				const self = this;
				self.$Utils.showToast('功能开发中','none')
			},
			
			getUserData() {
				var self = this;
				var postData = {};
				postData.tokenFuncName = 'getProjectToken';
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.userData = res.info.data[0]
					};
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					
				};
				postData.order = {
					listorder: 'desc'
				};
				postData.getBefore= {
					child:{
						tableName:'Label',
						middleKey:'parentid',
						key:'id',
						searchItem:{
							title:['in',['普通商品']]
						},
						condition:'in'
					}
				};
				postData.getAfter= {
					child:{
						tableName:'Label',
						middleKey:'id',
						key:'parentid',
						searchItem:{
							status:1
						},
						condition:'=',
						order:{
							listorder:'desc'
						}
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
						
					}
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			changeCurr(curr){
				const self = this;
				self.curr=curr;
				var idArray = [];
				self.mainData = [];
				if(curr!=-1){
					for (var i = 0; i < self.labelData[curr].child.length; i++) {
						idArray.push(self.labelData[curr].child[i].id)
					}
					self.searchItem.category_id = ['in',idArray];
					self.getMainData(true)
				}else{
					delete self.searchItem.category_id;
					self.getMainData(true)
				}
			},
			
			getLocation() {
				const self = this;
				const callback = (res) => {
					if (res) {
						console.log('res', res)
						if(res.authSetting){
							//self.$Utils.showToast('未授权位置信息，可在设置中开启','none')
							return
						};
						self.city = res.address_component.district
					};
				};
				self.$Utils.getLocation('reverseGeocoder', callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:"首页轮播"
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data[0]
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getNoticeData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:"首页广告"
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.noticeData = res.info.data[0]
					}
					self.$Utils.finishFunc('getNoticeData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getMainData(isNew) {
				var self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				var postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getLimit = ['content','bannerImg'];
				postData.order = {
					listorder: 'desc'
				};
				postData.getAfter = {
					sku:{
						tableName:'Sku',
						middleKey:'product_no',
						key:'product_no',
						searchItem:{
							status:1,
						},
						condition:'='
					}
				};
				var callback = function(res) {
					if (res.info.data.length > 0 && res.info.data[0]) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/proRow.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 140rpx;background: #f2f9ff;}	
	
	.headTop{height: 320rpx;background-color: #122433;margin-bottom: 80rpx;}
	.swiper-box {height: 280rpx;box-sizing: border-box;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item image{width: 100%;height: 100%;box-sizing: border-box;}
	
	/* 分类导航 */
	.indHome .item{width: 25%;text-align: center;color: #222;display: flex; flex-direction: column;line-height: 36rpx; }
	.indHome .item image{width:90rpx;height:90rpx; margin: 0 auto 20rpx auto;}
	
	/* 专区 */
	.zq-lable .item{width: 220rpx;height: 110rpx;padding-top: 16rpx;margin-right: 14rpx;border-radius: 10rpx;}
	.zq-lable .item .icon{width: 38rpx;height: 38rpx;position: absolute;right: 20rpx;bottom: 10rpx;}
	.zq-lable .item:nth-of-type(3n){margin-right: 0;}
	
	.orderNav .tt{width: auto;margin-right: 40rpx;}
	.orderNav .tt:last-child{margin-right: 0;}
	.orderNav .tt.on{font-size: 30rpx;font-weight: bold; color: #222;}
</style>
