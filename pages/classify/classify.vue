<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="d-flex j-sb a-start">
			<view class="text-center">
				<scroll-view class="leftNav scroll-row" scroll-y>
					<view class="tt" :class="curr==index?'on':''" v-for="(item,index) in navData" :key="index" @click="changeCurr(index)">{{item}}</view>
				</scroll-view>
			</view>
			
			
			<!-- 今日拼团 -->
			<view class="rightCont pr-3 pt-3">
				<!-- 类别 -->
				<view class="font-28 font-weight mb-2">类别</view>
				<view class="category font-26 pb-2 text-center d-flex a-start flex-wrap">
					<view class="item mb-2" v-for="(item,index) in categoryData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productList/productList'}})">
						<view class="pic rounded50 overflow-h"><image src="../../static/images/classificationl-img.png" mode=""></image></view>
						<view class="avoidOverflow pt-1">彩妆套装套装</view>
					</view>
				</view>
				
				<!-- 热门品牌 -->
				<view class="font-28 font-weight mb-2">热门品牌</view>
				<view class="category font-24 pb-1 text-center d-flex a-start flex-wrap">
					<view class="item mb-2" v-for="(item,index) in categoryData" :key="index">
						<view class="pic overflow-h"><image src="../../static/images/classificationl-img2.png" mode=""></image></view>
						<view class="avoidOverflow pt-1">香奈儿/CHANEL香奈儿</view>
					</view>
				</view>
				
				<!-- 商品推荐 -->
				<view class="font-28 font-weight mb-2">商品推荐</view>
				<view class="HotProduct d-flex j-sb flex-wrap">
					<view class="item mb-3" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
						<view class="pic rounded10 overflow-h"><image src="../../static/images/home-img2.png" mode=""></image></view>
						<view class="infor">
							<view class="tit font-26 d-flex a-center">
								<view class="avoidOverflow2">墨西哥牛油果8枚单果200g左右</view>
							</view>
							<view class="d-flex a-center mt-2">
								<view class="price font-30 font-weight mr-1">88</view>
								<view class="font-24">会员</view>
								<view class="VipPrice font-28"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view>
							</view>
						</view>
					</view>
				</view>
				
				<!-- 无数据 -->
				<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
			</view>
			
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
				is_show:false,
				curr:0,
				navData:['推荐品牌','户外运动','美妆个护','零食饮品','手机电脑'],
				categoryData:9,
				productData:4
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(index){
				const self = this;
				self.curr = index
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
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/productList.css";
	
	
	.leftNav{width: 180rpx;background-color: #F5F5F5;position: fixed;left:0;top: 88px;bottom: 0;}
	.leftNav .tt{width: 100%;height: 100rpx;line-height: 98rpx;border-bottom: 1px solid #fff;}
	.leftNav .tt:last-child{margin-right: 0;}
	.leftNav .tt.on{background-color: #fff;font-weight: bold;position: relative;}
	.leftNav .tt.on::before{content: '';width: 8rpx;height: 100%;background-color: #ffb2c2;position: absolute;left: 0;top: 0;bottom: 0;}
	.rightCont{padding-left: 210rpx;}
	
	/* 类别 */
	.category .item{width: 27.33%;margin: 0 3% 20rpx 3%; height: 150rpx;color: #333;}
	.category .item .pic{width: 100rpx;height: 100rpx;margin: 0 auto;background-color: #efefef;}
	
	
	.HotProduct .item{width: 245rpx;height: 398rpx;}
	.HotProduct .item .pic{width: 100%;height: 220rpx;background-color: efefef;}
	.HotProduct .item .infor{padding: 16rpx 0;}
</style>
