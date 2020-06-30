<template>
	<view>
		<view class="jg"></view>
		<view class="orderHead">
			<image src="../../static/images/icon-01.png" mode=""></image>
			<view class="bg-white p-3 font-28 color2">
				<view class="d-flex a-center pb-1">
					<view>张三</view>
					<view class="color9 font-24 pl-3">15689647582</view>
				</view>
				<view>陕西省西安市长安区韦曲街道</view>
			</view>
		</view>
		
		<view class="jg"></view>
		<view class="bg-white">
			<view class="pt-3">
				<view class="d-flex a-center j-sb px-3 pb-3">
					<view class="pic pr-2">
						<image src="../../static/images/details-img3.png" mode=""></image>
					</view>
					<view class="infor flex-1 d-flex flex-column j-sb">
						<view class="tit avoidOverflow">广州鹰嘴桃5斤 精品装</view>
						<view class="d-flex font-24 color6 mt-1">
							<view class="specsBtn mr-1">精装品5斤</view>
						</view>
						<view class="B-price d-flex a-center j-sb">
							<view class="d-flex a-center">
								<view class="price font-30 font-weight mr-2">79</view>
								<view class="font-24">会员</view>
								<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view>
							</view>
							<view class="font-26">×1</view>
						</view>
					</view>
				</view>
				<view class="d-flex a-center j-sb px-3 pb-3">
					<view class="pic pr-2">
						<image src="../../static/images/details-img3.png" mode=""></image>
					</view>
					<view class="infor flex-1 d-flex flex-column j-sb">
						<view class="tit avoidOverflow">广州鹰嘴桃5斤 精品装</view>
						<view class="d-flex font-24 color6 mt-1">
							<view class="specsBtn mr-1">精装品5斤</view>
						</view>
						<view class="B-price d-flex a-center j-sb">
							<view class="d-flex a-center">
								<view class="price font-30 font-weight mr-2">79</view>
								<view class="font-24">会员</view>
								<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view>
							</view>
							<view class="font-26">×1</view>
						</view>
					</view>
				</view>
			</view>
			<view class="line-h py-3 border-top d-flex font-28 color2 j-sb mx-3">
				<view>已支付</view>
				<view class="d-flex">
					<view class="font-27 color6">共1件商品</view>
					<view class="font-30 pl-2 font-weight price1">79</view>
				</view>
			</view>
			<view class="line-h py-3 border-top d-flex font-28 color2 j-sb mx-3">
				<view>订单编号</view>
				<view class="font-27 color6">515645134987465</view>
			</view>
		</view>
		
		<view class="jg"></view>
		<view class="bg-white pb-3">
			<view class="font-30 color2 pt-4 font-weight pl-3 line-h">物流信息</view>
			<view class="shadow font-26 color2 p-3 mx-3 mt-3 d-flex a-center j-sb wl">
				<view class="wlTxt">
					<view>物流公司：顺丰</view>
					<view class="pt-3">物流单号：SF265985625465</view>
					<view class="pt-3">包裹啥时候能到呀包裹啥时候能到呀</view>
				</view>
				<view @click="Router.navigateTo({route:{path:'/pages/userOrder-wuliu/userOrder-wuliu'}})"><image src="../../static/images/home-icon7.png"></image></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router
			}
		},
		methods: {
			
		}
	}
</script>

<style>
.jg{background-color: #f5f5f5;height: 20rpx;width: 100%;}
.orderHead image{width: 100%;height: 8rpx;}
.pic image{width: 160rpx;height: 160rpx;}
.infor{height: 160rpx;}
.specsBtn{background-color: #f5f5f5;}
.price1{color: #ed0000;}
.price1::before{content: '￥';}
.wl image{width: 20rpx;height: 30rpx;margin: 30rpx 0 30rpx 50rpx;}
.wlTxt{width: 460rpx;}
</style>
