<template>
	<view>
		
		<view class="userHead  main-bg-color">
			<view class="infor mx-3">
				<view class="d-flex a-center mt-4">
					<view><image class="photo" src="../../static/images/about-img.png" mode=""></image></view>
					<view style="width: 70%;">
						<view class="font-30 font-weight">哆啦A梦</view>
						<view class="d-flex mt-1">
							<view class="d-flex a-center font-24 rounded50" style="min-width: 150rpx;background-color: rgba(255,255,255,0.5);padding: 0rpx 6rpx; line-height: 40rpx;">
								<image style="width: 30rpx;height: 26rpx;" src="../../static/images/about-icon.png" mode=""></image>
								<view class="ml">普通会员</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="openVip rounded10 overflow-h px-3 py-2 mx-3 d-flex a-center j-sb" style="background-image: linear-gradient(to right,#534e4e,#191516);margin-top: -50rpx;">
			<view style="width: 70%;">
				<view class="font-26 d-flex a-center" style="color: #b89673;"><image style="width: 26rpx;height: 24rpx;margin-right: 6rpx;" src="../../static/images/about-icon1.png" mode=""></image>开通会员享全场成本价</view>
				<view class="font-22 mt-1" style="color: #c6a787;">开通试折惠会员享4大权益</view>
			</view>
			<view class="openBtn text-center text-white font-30 rounded50" @click="Router.navigateTo({route:{path:'/pages/VIP/VIP'}})">立即开通</view>
		</view>
		
		<view class="mx-3">
			<!-- 我的订单 -->
			<view class="userBox2 rounded10 bg-white font-24 px-3 mt-3">
				<view class="d-flex j-sb a-center py-3 border-bottom">
					<view class="font-30 font-weight">我的订单</view>
					<view class="more d-flex j-end a-center color9" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">查看全部<image class="arrowR" src="../../static/images/home-icon7.png" mode=""></image></view>
				</view>
				<view class="menu d-flex j-sb a-center color6 py-3 text-center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon2.png"></image>
						</view>
						<view>全部订单</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon3.png"></image>
						</view>
						<view>待发货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon4.png"></image>
						</view>
						<view>配送中</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon5.png"></image>
						</view>
						<view>已收货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<view class="icon">
							<image src="../../static/images/about-icon6.png"></image>
						</view>
						<view>已评价</view>
					</view>
				</view>
			</view>
			
			<!-- 我的服务 -->
			<view class="userBox2 sevice rounded10 bg-white my-3 font-24">
				<view class="d-flex j-sb a-center py-3  mx-3 border-bottom">
					<view class="font-30 font-weight">我的服务</view>
				</view>
				<view class="menu d-flex a-center color6 flex-wrap pt-3 text-center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder-myPingjia/userOrder-myPingjia'}})">
						<view class="icon">
							<image src="../../static/images/about-icon7.png"></image>
						</view>
						<view>我的评论</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="icon">
							<image src="../../static/images/about-icon8.png"></image>
						</view>
						<view>收货地址</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/aboutUs/aboutUs'}})">
						<view class="icon">
							<image src="../../static/images/about-icon9.png"></image>
						</view>
						<view>关于我们</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/storeLogin/storeLogin'}})">
						<view class="icon">
							<image src="../../static/images/about-icon10.png"></image>
						</view>
						<view>商家核销</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userPartner/userPartner'}})">
						<view class="icon">
							<image src="../../static/images/about-icon11.png"></image>
						</view>
						<view>品牌合作</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userProposal/userProposal'}})">
						<view class="icon">
							<image src="../../static/images/about-icon12.png"></image>
						</view>
						<view>投诉/建议</view>
					</view>
					<view class="item">
						<view class="icon">
							<image src="../../static/images/about-icon13.png"></image>
						</view>
						<view>客服</view>
					</view>
				</view>
			</view>
			
			<!-- 我的活动 -->
			<view class="userBox2 rounded10 bg-white font-24 px-3">
				<view class="d-flex a-center py-3 border-bottom">
					<view class="font-30 font-weight">我的活动</view>
				</view>
				<view class="menu d-flex j-sb a-center color6 py-3 text-center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-Looting/user-Looting'}})">
						<view class="icon">
							<image src="../../static/images/about-icon-01.png"></image>
						</view>
						<view>我的夺宝</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-mySeckill/user-mySeckill'}})">
						<view class="icon">
							<image src="../../static/images/about-icon-02.png"></image>
						</view>
						<view>我的秒杀</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/user-Collage/user-Collage'}})">
						<view class="icon">
							<image src="../../static/images/about-icon-03.png"></image>
						</view>
						<view>我的拼团</view>
					</view>
				</view>
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
					<image src="../../static/images/nabar5-a.png" />
				</view>
				<view class="text this-text">我的</view>
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
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	/* @import "../../assets/style/editInfor.css"; */
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	
	.userHead{width: 100%;height: 240rpx;}
	.userHead .photo{width: 110rpx;height: 110rpx;border-radius: 50%;margin-right: 20rpx;border: 1px solid #ffa278;background-color: #EEEEEE;}
	.userHead .infor{position: absolute;top: 0; right: 0; left: 0;}
	
	.openVip{height: 120rpx;}
	.openBtn{background-color: #cfa972;width: 160rpx;height: 60rpx;line-height: 60rpx;}
	
	.userBox2{width: 100%;height: 264rpx;position: relative; z-index: 1; }
	.userBox2 .item{flex: 1;}
	.userBox2 .item .icon{width: 48rpx;height:48rpx;display: block;margin: 0 auto;margin-bottom: 20rpx;position: relative;}
	.userBox2 .item .icon image{width: 100%;height: 100%;}
	
	.userBox2.sevice{height:390rpx ;overflow: hidden;}
	.userBox2.sevice .item{width: 25%; flex: initial; margin-bottom: 34rpx;}
	.userBox2.sevice .item .icon{margin-bottom: 16rpx;}
	
</style>
