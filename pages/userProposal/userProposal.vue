<template>
	<view>
		
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="font-26 px-3 py-3">
			<textarea v-model="submitData.content" placeholder="您对我们平台有什么好建议吗？" />
		</view>
		<view class="f5Bj-H10"></view>
		
		<view class="submitbtn pdtb15"  style="margin-top: 200rpx;">
			<button class="btn" type="button" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">提交</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					content:'',
					type:2
				}
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('留言成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.messageAdd(postData, callback);
			},
			


			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				
				if (pass) {	
					const callback = (user, res) => {
						console.log(res)
						self.messageAdd();
					};
					self.$Utils.getAuthSetting(callback);
				
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入留言内容', 'none')
				};
			},
		}
	};
</script>

<style>
	/* @import "../../assets/style/detail.css"; */
	page{padding-bottom: 60rpx;}
	textarea{font-size: 26rpx;width: 100%;height:400rpx;padding: 0;}
	
</style>
