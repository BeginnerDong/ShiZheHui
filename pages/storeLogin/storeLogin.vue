<template>
	<view>
		
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		
		<view class="d-flex j-center a-center" style="padding-top: 100rpx;">
			<view style="width: 243rpx;height: 258rpx;"><image src="../../static/images/the-loginl-img.png" mode=""></image></view>
		</view>
		
		
		<view class="loginCont">
			<view class="item px-3 d-flex a-center border rounded10">
				<view class="input">
					<input type="text" value="" placeholder="账号" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item px-3 d-flex a-center mt-4 border rounded10">
				<view class="input">
					<input type="password" value="" placeholder="密码" placeholder-class="placeholder">
				</view>
			</view>
			
			<view class="item submitbtn" style="padding: 0;border: 0;margin-top: 100rpx;" @click="Router.navigateTo({route:{path:'/pages/storeOrder/storeOrder'}})">
				<button class="Wbtn" type="submint">登录</button>
			</view>
		</view>
		
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				is_show: false,
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			navigateBack(){
				uni.navigateBack()
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
	
	page{padding-bottom: 60rpx;}	
	
	.loginCont{width:90%;margin: 0 auto;z-index: 2;background: #fff;padding: 0rpx 60rpx;box-sizing: border-box;padding-top: 100rpx;}
	.loginCont .item{height: 80rpx;}
	.loginCont .item .icon{width: 36rpx;height: 36rpx;margin-right: 30rpx;}
	.loginCont .item .input{width: 100%;}
	.loginCont .item .input input{font-size: 26rpx;height: 60rpx;}
</style>
