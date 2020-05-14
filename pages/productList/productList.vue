<template>
	<view>
		
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		
		<view class="RankingTit bg-white px-3 py-2 text-center d-flex a-center j-sb">
			<view class="tt flex-1 d-flex a-center j-center">
				<view class="d-flex a-center">
					<view>价格排行</view>
					<view class="arrowTB">
						<view class="arrowB">
							<image src="../../static/images/classificationl-icon1.png" mode=""></image>
						</view>
						<view class="arrowB mt">
							<image src="../../static/images/classificationl-icon2.png" mode=""></image>
						</view>
					</view>
				</view>
			</view>
			<view class="tt flex-1 d-flex a-center j-center">
				<view class="d-flex a-center">
					<view>销量排行</view>
					<view class="arrowTB">
						<view class="arrowB">
							<image src="../../static/images/classificationl-icon1.png" mode=""></image>
						</view>
						<view class="arrowB mt">
							<image src="../../static/images/classificationl-icon2.png" mode=""></image>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="HotProduct px-3 d-flex j-sb d-flex j-sb flex-wrap ">
			<view class="item rounded10 mt-3" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
				<view class="pic"><image src="../../static/images/home-img2.png" mode=""></image></view>
				<view class="infor">
					<view class="tit font-30 d-flex a-center">
						<view class="avoidOverflow2">墨西哥牛油果8枚单果200g左右</view>
					</view>
					<view class="d-flex a-center mt-2">
						<view class="price font-30 font-weight mr-2">88</view>
						<view class="font-24">会员</view>
						<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view>
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
				is_show: false,
				wx_info:{},
				is_show:false,
				productData:5
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
	/* @import "../../assets/style/orderNav.css"; */
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}	
	.orderNav{position: fixed; left:0; right: 0;top: 0; z-index: 10;}
	.orderNavH{height: 80rpx;}
	.orderNav .tt{width: auto;}
	.orderNav .tt.on{color: #222;font-size: 28rpx;font-weight: bold;}
	
	.arrowB{width: 16rpx;height: 8rpx;}
</style>
