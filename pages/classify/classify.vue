<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="d-flex j-sb a-start">
			<view class="text-center">
				<scroll-view class="leftNav scroll-row" scroll-y>
					<view class="tt" :class="curr==-1?'on':''" 
					@click="changeCurr(-1)">推荐品牌</view>
					<view class="tt" :class="curr==index?'on':''" v-for="(item,index) in labelData" :key="index" 
					@click="changeCurr(index)">{{item.title}}</view>
				</scroll-view>
			</view>
			
			
			<!-- 今日拼团 -->
			<view class="rightCont pr-3 pt-3 w-100">
				<!-- 类别 -->
				<view class="font-28 font-weight mb-2" v-if="curr!=-1&&labelTwoData.length>0">类别</view>
				<view class="category font-26 pb-2 text-center d-flex a-start flex-wrap" v-if="curr!=-1&&labelTwoData.length>0">
					<view class="item mb-2" v-for="(item,index) in labelTwoData" :key="index" :data-id="item.id"
					@click="Router.navigateTo({route:{path:'/pages/productList/productList?id='+$event.currentTarget.dataset.id}})">
						<view class="pic rounded50 overflow-h"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode="">
						</image></view>
						<view class="avoidOverflow pt-1">{{item.title}}</view>
					</view>
				</view>
				
				<!-- 热门品牌 -->
				<view class="font-28 font-weight mb-2">热门品牌</view>
				<view class="category font-24 pb-1 text-center d-flex a-start flex-wrap">
					<view class="item mb-2" v-for="(item,index) in brandData" :key="index">
						<view class="pic overflow-h"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="avoidOverflow pt-1">{{item.title}}</view>
					</view>
				</view>
				
				<!-- 商品推荐 -->
				<view class="font-28 font-weight mb-2">商品推荐</view>
				<view class="HotProduct d-flex j-sb flex-wrap">
					<view class="item mb-3" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic rounded10 overflow-h"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
						<view class="infor">
							<view class="tit font-26 d-flex a-center">
								<view class="avoidOverflow2">{{item.title}}</view>
							</view>
							<view class="d-flex a-center mt-2">
								<view class="price font-30 font-weight mr-1">{{item.price}}</view>
								<!-- <view class="font-24">会员</view>
								<view class="VipPrice font-28"><image class="arrow" src="../../static/images/home-icon6.png" mode="">
								</image>￥{{item.member_price}}</view> -->
							</view>
							<view class="d-flex a-center mt-2">
								<view class="font-24">会员</view>
								<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥{{item.member_price}}</view>
							</view>
						</view>
					</view>
				</view>
				<!-- 无数据 -->
				<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
			</view>
			
		</view>
		<view style="width: 100%;height: 110rpx;"></view>
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
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">分类</view>
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
				is_show: false,
				wx_info:{},
				curr:-1,
				categoryData:9,
				productData:4,
				labelData:[],
				brandData:[],
				searchItem:{
					thirdapp_id:2,
					type:1
				},
				mainData:[],
				labelTwoData:[],
				idArray:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			console.log(options)
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			if(options.index){
				self.index = options.index;
			};
			self.$Utils.loadAll(['getLabelData','getBrandData'], self);
		},
		
		methods: {
			
			changeCurr(index){
				const self = this;
				self.mainData = [];
				self.labelTwoData = [];
				self.idArray = [];
				console.log('index',index)
				console.log('self.curr',self.curr)
				if(self.curr!=index){
					console.log('123',323)
					self.curr = index
					if(index!=-1){
						console.log('12',323)
						self.getLabelTwoData()
					}else{
						delete self.searchItem.category_id;
						self.getMainData(true)
					}
				}
			},
			
			getLabelTwoData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:3,
					parentid:self.labelData[self.curr].id
				};
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelTwoData.push.apply(self.labelTwoData, res.info.data);
						for (var i = 0; i < self.labelTwoData.length; i++) {
							self.idArray.push(self.labelTwoData[i].id)
						}
						self.searchItem.category_id = ['in',self.idArray]
						self.getMainData(true)
					}
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getBrandData() {
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
						middleKey:'parentid',
						key:'id',
						searchItem:{
							status:['in',1],
							title:['in','热门品牌']
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.brandData.push.apply(self.brandData, res.info.data);
						
					}
					self.$Utils.finishFunc('getBrandData');
				};
				self.$apis.labelGet(postData, callback);
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
						if(self.index){
							//self.curr = self.index;
							self.changeCurr(self.index)
						}else{
							self.getMainData()
						};
					}
					self.$Utils.finishFunc('getLabelData');
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
				postData.order = {
					sale_count: 'desc'
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
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/productList.css";
	
	
	.leftNav{width: 180rpx;background-color: #F5F5F5;position: fixed;left:0;top: 0;bottom: 0;}
	.leftNav .tt{width: 100%;height: 100rpx;line-height: 98rpx;border-bottom: 1px solid #fff;}
	.leftNav .tt:last-child{margin-right: 0;}
	.leftNav .tt.on{background-color: #fff;font-weight: bold;position: relative;}
	.leftNav .tt.on::before{content: '';width: 8rpx;height: 100%;background-color: #ffb2c2;position: absolute;left: 0;top: 0;bottom: 0;}
	.rightCont{padding-left: 210rpx;}
	
	/* 类别 */
	.category .item{width: 27.33%;margin: 0 3% 20rpx 3%; height: 150rpx;color: #333;}
	.category .item .pic{width: 100rpx;height: 100rpx;margin: 0 auto;background-color: #efefef;}
	
	
	/* .HotProduct .item{width: 245rpx;height: 398rpx;} */
	.HotProduct .item{width: 245rpx;}
	.HotProduct .item .pic{width: 100%;height: 220rpx;background-color: efefef;}
	.HotProduct .item .infor{padding: 16rpx 0;}
</style>
