<template>
	<view>
		<view class="bg-white px-3">
			<view class="py-4 line-h border-bottom d-flex a-center j-sb">
				<view class="font-30 color2">姓名</view>
				<view class="font-26 color9 text-right"><input type="text" v-model="submitData.name"  placeholder="请输入" /></view>
			</view>
			<!-- <view class="py-4 line-h border-bottom d-flex a-center j-sb">
				<view class="font-30 color2">年龄</view>
				<view class="font-26 color9 text-right"><input type="text" v-model="submitData.age" placeholder="请输入" /></view>
			</view> -->
			<view class="py-4 line-h border-bottom d-flex a-center j-sb">
				<view class="font-30 color2">性别</view>
				<view class="font-26 color2 d-flex">
					<view class="d-flex mr-4 a-center" @click="changeGender(1)">
						<image :src="submitData.gender==1?'../../static/images/icon-02.png':'../../static/images/icon-o3.png'" class="sexIcon"></image>
						<view>男</view>
					</view>
					<view class="d-flex ml-4 a-center" @click="changeGender(0)">
						<image :src="submitData.gender==0?'../../static/images/icon-02.png':'../../static/images/icon-o3.png'" class="sexIcon"></image>
						<view>女</view>
					</view>
				</view>
			</view>
			<view class="py-4 line-h border-bottom d-flex a-center j-sb">
				<view class="font-30 color2">电话</view>
				<view class="font-26 color9 text-right"><input type="number" maxlength="11" v-model="submitData.phone" 
				placeholder="请输入" /></view>
			</view>
		</view>
		
		<view class="font-30 mx-3 rounded10 btn" @click="Utils.stopMultiClick(submit)">确定</view>
	</view>
</template>

<script>
	export default {
		
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				submitData:{
					name:'',
					//age:'',
					gender:1,
					phone:''
				}
			}
		},
		
		methods: {
			
			changeGender(type){
				const self = this;
				self.submitData.gender = type;
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var NewObejct = self.$Utils.cloneForm(self.submitData);
				delete NewObejct.gender;
				const pass = self.$Utils.checkComplete(NewObejct);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.userInfoUpdate();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate() {
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
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('保存成功', 'none');
						uni.setStorageSync('infoOk', true);
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
		}
	}
</script>

<style>
page{background-color: #f5f5f5;}
.sexIcon{width: 32rpx;height: 32rpx;margin-right: 10rpx;}
.btn{line-height: 80rpx;text-align: center;color: #fff;background-color: #FFB2C2;margin-top: 60rpx;}
</style>
