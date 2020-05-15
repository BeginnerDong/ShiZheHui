<template>
	<view>
		<!-- <view class="border-bottom position-fixed left-0 right-0 top0"></view> -->
		<view class="topNavFix bg-white">
			<view class="orderNav d-flex j-sb a-center shadow color6">
				<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部</view>
				<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">进行中</view>
				<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">成功</view>
				<view class="tt" :class="curr==4?'on':''" @click="changeCurr('4')">失败</view>
			</view>
		</view>
		<view class="topNavH"></view>
		
		<view class="mt-3 mx-3">
			<view class="proRow">
				<view class="item mb-3 bg-white" v-for="(item,index) in orderData" :key="index">
					<view class="priList" >
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="d-flex a-center" v-if="curr==1||curr==2">
								还差一人<view class="red ml-1">进行中</view>
							</view>
							<view class="red" v-if="curr==3">成功拼团</view>
							<view class="d-flex a-center" v-if="curr==4">
								还差一人<view class="red ml-1">拼团失败</view>
							</view>
						</view>
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
										<view class="red d-flex a-center font-24">团购价<view class="font-30 font-weight mr-2">￥88</view></view>
										
										<view class="yuanJia">121</view>
									</view>
									<view class="font-26">×1</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="R-fixIcon"><image src="../../static/images/looting-icon.png" mode="" @click="Router.navigateTo({route:{path:'/pages/user-CollageOrder/user-CollageOrder'}})"></image></view>
		
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
				orderData:1,
				currNav:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			currNavChange(currNav){
				const self = this;
				if(currNav!= self.currNav){
					self.currNav = currNav
				}
			},
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height:80rpx;position: fixed;top: 0rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height:80rpx;}
	
	.proRow .item{padding: 0 30rpx;}
	.proRow .item .priList{padding: 30rpx 0;border-top: 1px solid #eee;}
	.proRow .item .priList:first-child{border-top: 0;}	
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	.orderNav .tt.on::after{bottom: 0;}
	
</style>
