<template>
	<view>
		<view class="position-fixedXY bg"><image src="../../static/images/img-o1.png" mode=""></image></view>
		<view class="font-32 conBox text-center">
			<view>推广二维码</view>
			<image src="../../static/images/img-02.png" mode=""></image>
			<view class="font-30 saveBtn">保存到相册</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
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
