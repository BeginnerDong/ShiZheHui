<template>
	<view>
		
		<view class="topNavFix bg-white">
			<view class="d-flex a-center px-3 pt-2 pb-1">
				<view class="photo overflow-h"><image src="../../static/images/a-bulkl-img.png" mode=""></image></view>
				<view class="ml-2 font-30">哆啦A梦</view>
			</view>
			<view class="orderNav bg-white d-flex j-sb a-center shadow color6">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">待核销</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已核销</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view class="mt-3 mx-3">
			<view class="proRow">
				<view class="item mb-3">
					<view class="font-24 d-flex j-sb a-center mb-2">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red" v-if="curr==1||curr==2">待核销</view>
						<view class="red" v-if="curr==3">已核销</view>
					</view>
					<view class="priList pb-3"  v-for="(item,index) in orderData" :key="index">
						<view class="d-flex a-center j-sb">
							<view class="pic">
								<image src="../../static/images/shopping-icon4.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow">墨西哥牛油果8枚单果200g左右</view>
								<view class="d-flex font-24 color6 mt-1">
									<view class="specsBtn mr-1">精装品5斤</view>
								</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="d-flex a-center">
										<view class="price font-30 font-weight mr-2">88</view>
										<view class="font-24">会员</view>
										<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view>
									</view>
									<view class="font-26">×1</view>
								</view>
							</view>
						</view>
					</view>
					<view class="pb-3 d-flex a-center j-sb font-26 pt-3 border-top">
						<view>客户信息：</view>
						<view class="d-flex a-center"><span class="mr-2">张丹</span>15623562666</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="R-fixIcon"><image src="../../static/images/merchantsl-icon.png" mode="" @click="Router.navigateTo({route:{path:'/pages/storeOrder-detail/storeOrder-detail'}})"></image></view>
		
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
				score:'',
				wx_info:{},
				is_show:false,
				curr:1,
				orderData:2
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height: 220rpx;position: fixed;top:88rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height: 220rpx;}
	
	.proRow .item{padding: 20rpx 30rpx 0 30rpx;}
	.orderNav .tt.on::after{bottom: 0;}
	
	.photo{width: 110rpx;height: 110rpx;border-radius: 50%;background-color: #eee;}
</style>
