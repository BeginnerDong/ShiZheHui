<template>
	<view v-if="showAll">
		
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		
		<view class="d-flex j-center a-center" style="padding-top: 100rpx;">
			<view style="width: 243rpx;height: 258rpx;"><image src="../../static/images/the-loginl-img.png" mode=""></image></view>
		</view>
		
		
		<view class="loginCont">
			<view class="item px-3 d-flex a-center border rounded10">
				<view class="input">
					<input type="text" v-model="submitData.login_name" placeholder="账号" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item px-3 d-flex a-center mt-4 border rounded10">
				<view class="input">
					<input type="password" v-model="submitData.password" placeholder="密码" placeholder-class="placeholder">
				</view>
			</view>
			
			<view class="item submitbtn" style="padding: 0;border: 0;margin-top: 100rpx;"  @click="submit">
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
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad() {
			const self = this;
			uni.hideLoading();
			if (uni.getStorageSync('merchantToken')) {
				uni.redirectTo({
					url: '/pages/storeOrder/storeOrder'
				})
			}else{
				self.showAll = true
			}
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('merchantToken', res.token);
							uni.setStorageSync('merchantInfo', res.info);
							setTimeout(function() {
								self.Router.reLaunch({route:{path:'/pages/storeOrder/storeOrder'}})
							}, 1000);
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.shopLogin(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
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
