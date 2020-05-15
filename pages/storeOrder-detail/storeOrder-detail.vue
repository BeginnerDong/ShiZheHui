<template>
	<view>
		
		<view class="mt-3 mx-3">
			<view class="proRow">
				<view class="item mb-3">
					<view class="font-24 d-flex j-sb a-center mb-2">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red">待取货</view>
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
		
			<view class="px-3 py-3 mt-3 d-flex a-center j-sb font-26 bg-white">
				<view>客户信息：</view>
				<view class="d-flex a-center"><span class="mr-2">张丹</span>15623562666</view>
			</view>
			
		</view>
		
		<view class="submitbtn" style="margin-top: 300rpx;" @click="Router.navigateTo({route:{path:'/pages/storeOrder/storeOrder'}})">
			<button class="btn" type="submint">确定核销</button>
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
				mainData:[{},{},{}],
				is_popupShow:false,
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
			}
		}
	};
</script>

<style>
	@import "../../assets/style/proRow.css";
	
	page{background: #F5F5F5;padding-bottom: 120rpx;}
	
	
	
	.samllEwm{width: 78rpx;height: 78rpx;}
	
	.FixB-btn{position: fixed;left: 0;right: 0;bottom: 0;height: 80rpx;line-height: 80rpx;}
</style>
