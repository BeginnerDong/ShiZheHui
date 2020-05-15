<template>
	<view>
		
		<view class="detailBanner position-relative">
			<swiper class="swiper-box rounded10 overflow-h" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-color="#adadad" indicator-active-color="#ffbcd7">
				<block v-for="(item,index) in labelData" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
			<view class="text-center text-white position-absoluteXY font-34" style="height: 72rpx;padding: 14rpx 30rpx;bottom: auto;background-color: rgb(0,0,0,0.6);">23:20:18</view>
		</view>
		<view class="pl-3 py-2 main-bg-color d-flex a-center j-sb detail-priceBox text-white" style="height: auto;">
			<view style="width: 85%;">
				<view class="d-flex a-center font-24">
					<view>秒杀价<span class="price font-weight">79</span></view>
					<view class="ml-3 pt-1">原价<span class="font-28 font-weight">￥121</span></view>
				</view>
				<!-- <view class="d-flex a-center font-22 mt-1 pt">
					<view>库存：235</view>
					<view class="ml-5">销量：230</view>
				</view> -->
			</view>
			<view class="shareBtn d-flex j-center a-center font-24"><image class="shareIcon" src="../../static/images/details-icon3.png" mode=""></image><view class="ml-1">分享</view></view>
		</view>
		<view class="mx-3 py-3">
			<view class="font-32 font-weight pb-1">形象美奇异果洁净嫩滑洁面补水保湿洗面奶护肤品水果去角质洗面奶</view>
			<view class="d-flex font-24 color6 mb-1">
				<view class="text-secondary">全新 | L(170/88A)码</view>
			</view>
			<view class="font-26 color9">新品首发金夫人了都是感觉会根据开发商的华工科技粉红色关键人火锅积分兑换</view>
		</view>
		
		<view class="f5Bj-H20"></view>
		<view class="mx-3 pt-3">
			<view class="d-flex a-center j-sb">
				<view class="color6">商品评论</view>
				<view class="d-flex j-end a-center font-24" @click="Router.navigateTo({route:{path:'/pages/product-PingJia/product-PingJia'}})">查看全部<image class="arrowR" src="../../static/images/home-icon7.png" mode=""></image></view>
			</view>
			<view class="detail_pjLis">
				<view class="item" style="border-bottom: none;">
					<view class="d-flex j-sb a-center pb-1 font-24">
						<view class="d-flex a-center">
							<view class="photo mr-1"><image src="../../static/images/details-img2.png" mode=""></image></view>
							<view class="name color6">哆啦A梦</view>		
						</view>
						<view class="time color9">2020-03-17</view>
					</view>
					<view class="text font-26 pt-1">根据瑞灵活赶紧来都搞好了电话沟通各环节的反馈给换了多个黑客帝国</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		<view class="px-3">
			<view class="py-3">
				<view class="font-30 color6 mb-3">详情介绍</view>
				<view class="xqInfor">
					<view class="cont fs14 text-center">
						<view>管理客服电话</view>
						<view>悲愤交加鹤骨鸡肤供货</view>
						<view>方点击可供货方都拉黑干活的放假</view>
						<view>价格过节费考虑到加工费</view>
						<view><image src="../../static/images/details-img1.png" mode="widthFix"></image></view>
						<view>管理客服电话</view>
						<view>悲愤交加鹤骨鸡肤供货</view>
						<view>方点击可供货方都拉黑干活的放假</view>
						<view>价格过节费考虑到加工费</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<view class="xqbotomBar center pl-3">
			<view class="d-flex fs12">
				<view class="ite flexColumn" style="width: 90rpx;" @click="navigateBack">
					<view class="icon"><image src="../../static/images/details-icon4.png" mode=""></image></view>
					<view class="mt-1">返回</view>
				</view>
			</view>
			<view style="width: 600rpx;">
				<view class="d-flex a-center j-sb" v-show="payState">
					<view class="BarState d-flex a-center j-center text-center">
						<view class="font-26">支付倒计时：00:20:30</view>
					</view>
					<view class="payBtn text-white font-30 main-bg-color" style="width: 280rpx;" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">立即支付</view>
				</view>
				<view class="d-flex a-center j-sb" v-show="payState">
					<view class="BarState d-flex a-center j-center text-center">
						<view class="numBox d-flex">
							<view class="btn" @click="counter('-')">-</view>
							<view class="num mx-1">{{count}}</view>
							<view class="btn pubBj white add" @click="counter('+')">+</view>
						</view>
					</view>
					<view class="payBtn text-white font-30 main-bg-color" style="width: 280rpx;">立即出价</view>
				</view>
				<view class="BarState d-flex a-center j-center text-center pr-4" v-show="!payState">
					<image style="width: 68rpx;height: 75rpx;" src="../../static/images/indianal-icon3.png" mode=""></image>
					<view class="font-28 ml-2">该商品已经被抢走了</view>
				</view>
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
				count:1,
				payState:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			navigateBack(){
				uni.navigateBack({
				});
			},
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
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
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{padding-bottom:140rpx;}
	
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
	
	/* .BarState{width: 310rpx;} */
	.numBox{border: 0;}
	.numBox .btn{width: 60rpx;height: 60rpx;border: 0;line-height: 60rpx;background-color: #F5F5F5;border-radius: 50%;font: 44rpx;color: #222;}
	.numBox view.num{width: 120rpx;height: 60rpx;line-height: 60rpx;border-radius: 4rpx;border: 0;background-color: #F5F5F5;color: ##e90000;}
</style>
