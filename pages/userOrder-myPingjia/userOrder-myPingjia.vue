<template>
	<view>
		
		<view class="mx-3">
			<view class="proRow">
				<view class="item mt-3 bg-white"  v-for="(item,index) in mainData" :key="index">
					<view>
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已评价</view>
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
					<view class="f5Text f5bj rounded10 p-2 font-26 color6 mt-2">
						<view>很反感娄山关路是个凡人的洛杉矶经历过购房即可看到据国家风口浪尖加工费考虑到国家反馈的结果了</view>
					</view>
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
				score:'',
				wx_info:{},
				mainData:3
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
	page{background: #F5F5F5;padding-bottom:30rpx;}
	
	.f5Text{line-height: 42rpx;}
</style>
