<template>
	<view>
		<view class="position-fixedXY bg"><image src="../../static/images/img-o1.png" mode=""></image></view>
		<view class="font-32 conBox text-center">
			<view>推广二维码</view>
			<image :src="QrData.url?QrData.url:''" mode=""></image>
			<view class="font-30 saveBtn" @click="save">保存到相册</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				QrData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: wx.getStorageSync('user_info').user_no,
					page: 'pages/index/index',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.QrData = res.info;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.getQrCode(postData, callback);
			},
			
			save() {
				let self = this
				//若二维码未加载完毕，加个动画提高用户体验
				wx.showToast({
					icon: 'loading',
					title: '正在下载图片',
					duration: 1000
				})
				//判断用户是否授权"保存到相册"
				wx.getSetting({
					success(res) {
						//没有权限，发起授权
						if (!res.authSetting['scope.writePhotosAlbum']) {
							wx.authorize({
								scope: 'scope.writePhotosAlbum',
								success() { //用户允许授权，保存图片到相册
									self.savePhoto();
								},
								fail() { //用户点击拒绝授权，跳转到设置页，引导用户授权
								console.log('fail')
									wx.showToast({
										icon: 'none',
										title: '请至小程序设置中开启相册权限',
										duration: 1000
									})
								}
							})
						} else { //用户已授权，保存到相册
							self.savePhoto()
						}
					}
				})
			},
			//保存图片到相册，提示保存成功
			savePhoto() {
				let self = this
				wx.downloadFile({
					url: self.QrData.url,
					success: function(res) {
						wx.saveImageToPhotosAlbum({
							filePath: res.tempFilePath,
							success(res) {
								wx.showToast({
									title: '保存成功',
									icon: "success",
									duration: 1000
								})
							}
						})
					}
				})
			},
			
		}
	}
</script>

<style>
page{background-color: #FFEFF1;}
.bg image{height: 1049rpx;width: 100%;}
.conBox{margin-top: 425rpx;position: relative;color: #fff;}
.conBox image{width: 400rpx;height: 400rpx;margin: 100rpx auto;}
.saveBtn{color: #fffefe;background-color: #FFB2C2;line-height: 80rpx;width: 520rpx;border-radius: 40rpx;margin: 0 auto;text-align: center;margin-top: 120rpx;}
</style>
