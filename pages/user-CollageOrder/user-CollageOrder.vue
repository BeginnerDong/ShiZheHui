<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="topNavFix bg-white">
			<view class="tooling_indNav color6 mt-3 mb-2 mx-4">
				<view class="list rounded50 d-flex text-center j-sb f5bj">
					<view class="tt" :class="currNav==1?'on':''" @click="currNavChange('1')">快递包邮</view>
					<view class="tt" :class="currNav==2?'on':''" @click="currNavChange('2')">到店自提</view>
				</view>
			</view>
			<view class="orderNav d-flex j-sb a-center shadow color6" v-show="currNav==1">
				<view class="tt" :class="stateOne==1?'on':''" @click="changeStateOne('1')">全部</view>
				<view class="tt" :class="stateOne==2?'on':''" @click="changeStateOne('2')">待发货</view>
				<view class="tt" :class="stateOne==3?'on':''" @click="changeStateOne('3')">配送中</view>
				<view class="tt" :class="stateOne==4?'on':''" @click="changeStateOne('4')">已收货</view>
				<view class="tt" :class="stateOne==5?'on':''" @click="changeStateOne('5')">已评论</view>
			</view>
			<view class="orderNav d-flex j-sb a-center shadow color6" v-show="currNav==2">
				<view class="tt" :class="stateTwo==1?'on':''" @click="changeStateTwo('1')">全部</view>
				<view class="tt" :class="stateTwo==2?'on':''" @click="changeStateTwo('2')">待取货</view>
				<view class="tt" :class="stateTwo==3?'on':''" @click="changeStateTwo('3')">已核销</view>
				<view class="tt" :class="stateTwo==4?'on':''" @click="changeStateTwo('4')">已评论</view>
			</view>
		</view>
		
		<view class="topNavH"></view>
		
		<view class="mt-3 mx-3">
			<view class="proRow"  v-show="currNav==1">
				<view class="item mb-3 bg-white">
					<view class="priList" v-for="(item,index) in orderData" :key="index">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red" v-if="stateOne==1||stateOne==2">待发货</view>
							<view class="red" v-if="stateOne==3">配送中</view>
							<view class="red" v-if="stateOne==4">已收货</view>
							<view class="red" v-if="stateOne==5">已评价</view>
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
					<view class="underBtn d-flex j-end a-center pb-3" v-if="stateOne==3">
						<view class="Bbtn red">确认收货</view>
					</view>
					<view class="underBtn d-flex j-end a-center pb-3"  v-if="stateOne==4">
						<view class="Bbtn red" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaList/userOrder-pingjiaList'}})">去评价</view>
					</view>
					<view class="pb-3"  v-if="stateOne==5">
						<view class="px-3 f5bj rounded10 py-2 font-26 color6">路费及人事法规偶然时间管理认识就结果反馈了登记管理发的寄过来发动机就开了家</view>
					</view>
				</view>
			</view>
			
			<view class="proRow"  v-show="currNav==2">
				<view class="item mb-3 bg-white">
					<view class="priList"  v-for="(item,index) in orderData" :key="index">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red" v-if="stateTwo==1||stateTwo==2">待取货</view>
							<view class="red" v-if="stateTwo==3">已核销</view>
							<view class="red" v-if="stateTwo==4">已评论</view>
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
					<view class="pb-3 d-flex a-center j-sb font-26" v-if="stateTwo==1||stateTwo==2">
						<view>核销码：</view>
						<view class="hxEwm"  @click="hxEwmShow"><image src="../../static/images/ma-img.png" mode=""></image></view>
					</view>
					<view class="underBtn d-flex j-end a-center pb-3"  v-if="stateTwo==3">
						<view class="Bbtn red" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjia/userOrder-pingjia'}})">去评价</view>
					</view>
					<view class="pb-3" v-if="stateTwo==4">
						<view class="px-3 f5bj rounded10 py-2 font-26 color6">路费及人事法规偶然时间管理认识就结果反馈了登记管理发的寄过来发动机就开了家</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<!-- 核销码放大弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="hxEwmShow" v-show="is_hxEwmShow">
			<view><image class="ewm" src="../../static/images/ma-img1.png" mode=""></image></view>
			<view class="closeBtn" @click="hxEwmShow">×</view>
		</view>
		
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
				stateOne:1,
				orderData:2,
				currNav:1,
				is_hxEwmShow:false,
				stateTwo:1
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
			changeStateOne(stateOne){
				const self = this;
				if(stateOne!= self.stateOne){
					self.stateOne = stateOne
				}
			},
			changeStateTwo(stateTwo){
				const self = this;
				if(stateTwo!= self.stateTwo){
					self.stateTwo = stateTwo
				}
			},
			hxEwmShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_hxEwmShow = !self.is_hxEwmShow
			}
			
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	/* @import "../../assets/style/editInfor.css"; */
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height:200rpx;position: fixed;top:0rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height:200rpx;}
	
	.proRow .item{padding: 0 30rpx;}
	.proRow .item .priList{padding: 30rpx 0;border-top: 1px solid #eee;}
	.proRow .item .priList:first-child{border-top: 0;}	
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	.orderNav .tt.on::after{bottom: 0;}
	
	.hxEwm{width: 80rpx;height: 80rpx;}
	
	.hxEwmShow{width: 70%;position: fixed;top: 45%;left: 50%;transform: translate(-50%,-50%);z-index: 50;}
	.hxEwmShow .ewm{width: 440rpx;height: 440rpx;display: block; margin: 0 auto;}
</style>
