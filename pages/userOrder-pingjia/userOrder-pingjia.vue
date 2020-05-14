<template>
	<view>
		<view class="mt-3 mx-3">
			<view class="proRow ">
				<view class="item mb-3 bg-white">
					<view class="priList">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已收货</view>
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
										<view class="price font-30 font-weight mr-2">88</view>
										<view class="font-24">会员</view>
										<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view>
									</view>
									<view class="font-26">×1</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="bg-white rounded10 px-3 py-3">
				<view class="font-26">
					<textarea value="" placeholder="请填写商品评论,让更多人知道" />
				</view>
				<view class="d-flex a-center">
					<view class="d-flex a-center flex-wrap onloadbtn">
						<image src="../../static/images/commental-img.png" mode=""></image>
					</view>
					<view class="font-24 color9 ml-2">可上传多张照片</view>
				</view>
				<view class="d-flex a-start flex-wrap pjPic">
					<image src="../../static/images/to-the-pointl-img.png" mode=""></image>
					<image src="../../static/images/to-the-pointl-img.png" mode=""></image>
					<image src="../../static/images/to-the-pointl-img.png" mode=""></image>
				</view>
				
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 120rpx;">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">确定</button>
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
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
		}
	};
</script>

<style>
	
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	textarea{font-size: 26rpx;width: 100%;height:240rpx;padding: 0;}
	.onloadbtn{width: 120rpx;height: 120rpx;}
	.pjPic image{width: 142rpx;height: 142rpx;margin: 20rpx 20rpx 0 0;border-radius: 10rpx;}
	.pjPic image:nth-of-type(4n){margin-right: 0;}
</style>
