<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<!-- 搜索 -->
		<view class="seachbox d-flex a-center j-sb bg-white px-3 py-3"> 
			<view class="d-flex a-center rr f5bj" style="width:100%;" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<button class="seachBtn" type="button"><image src="../../static/images/home-icon1.png" mode=""></image></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="搜索您想要的商品" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<view class="mx-3 mt-3">
			<view class="proRow">
				<view class="item d-flex a-start j-sb mt-3" >
					<view class="pic" @click="Router.navigateTo({route:{path:'/pages/OneYuanDetail/OneYuanDetail'}})">
						<image src="../../static/images/details-img3.png" mode=""></image>
						<view class="picText px-3">02:00:03</view>
					</view>
					<view class="infor">
						<view class="avoidOverflow font-30">12色可水洗油画棒 水溶性旋转涂料板</view>
						<view class="d-flex font-24 color6 mt-1">
							<view class="text-secondary">全新 | L(170/88A)码</view>
						</view>
						<view class="B-price">
							<view class="d-flex a-center j-sb">
								<view class="d-flex a-center mt-1">
									<view class="red font-24">当前价<span class="price">79</span></view>
									<view class="yuanJia ml-2">121</view>
								</view>
								<view class="stateBtn stateOne main-bg-color d-flex a-center j-center" @click="Router.navigateTo({route:{path:'/pages/OneYuanDetail/OneYuanDetail'}})"><image class="icon" src="../../static/images/indianal-icon.png" mode=""></image>出价</view>
							</view>
							
						</view>
					</view>
				</view>
				<view class="item d-flex a-start j-sb mt-3" >
					<view class="pic" @click="Router.navigateTo({route:{path:'/pages/OneYuanDetail/OneYuanDetail'}})">
						<image src="../../static/images/details-img3.png" mode=""></image>
						<view class="picText px-3">已售空</view>
					</view>
					<view class="infor">
						<view class="avoidOverflow font-30">12色可水洗油画棒 水溶性旋转涂料板</view>
						<view class="d-flex font-24 color6 mt-1">
							<view class="text-secondary">全新 | L(170/88A)码</view>
						</view>
						<view class="B-price">
							<view class="d-flex a-center j-sb">
								<view class="d-flex a-center mt-1">
									<view class="red font-24">当前价<span class="price">79</span></view>
									<view class="yuanJia ml-2">121</view>
								</view>
								<view class="stateBtn stateTwo d-flex a-center j-center" @click="Router.navigateTo({route:{path:'/pages/OneYuanDetail/OneYuanDetail'}})"><image class="icon" src="../../static/images/indianal-img.png" mode=""></image>获拍</view>
							</view>
						</view>
					</view>
				</view>
				<view class="item d-flex a-start j-sb mt-3" >
					<view class="pic" @click="Router.navigateTo({route:{path:'/pages/pintuanlDetail/pintuanlDetail'}})">
						<image src="../../static/images/details-img3.png" mode=""></image>
						<view class="picText px-3">已获拍</view>
					</view>
					<view class="infor">
						<view class="avoidOverflow font-30">12色可水洗油画棒 水溶性旋转涂料板</view>
						<view class="d-flex font-24 color6 mt-1">
							<view class="text-secondary">全新 | L(170/88A)码</view>
						</view>
						<view class="B-price">
							<view class="d-flex a-center j-sb">
								<view class="d-flex a-center mt-1">
									<view class="red font-24">当前价<span class="price">79</span></view>
									<view class="yuanJia ml-2">121</view>
								</view>
								<view class="stateBtn stateThree d-flex a-center j-center" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">去支付</view>
							</view>
						</view>
					</view>
				</view>
				
			</view>
		</view>
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
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
				seckillData:3
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proRow.css";
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}
	
	.proRow .item .pic{position: relative;}
	.picText{height: 44rpx;line-height: 44rpx;background-image: linear-gradient(to bottom,rgba(0,0,0,0),rgba(0,0,0,0.5));text-align: center;position: absolute;left: 0;right: 0;bottom: 0; color: #fff;font-size: 24rpx;}
	
	.stateBtn{min-width: 110rpx;height: 50rpx;padding: 0 6rpx;border-radius: 30rpx;font-size: 24rpx;color: #fff;}
	.stateBtn .icon{width: 34rpx;height: 34rpx;margin-right: 4rpx;}
	
	.stateOne{background-color: #FFB2C2;}
	.stateTwo{background-color: #bdbdbd;}
	.stateThree{background-color: #222;}
</style>

