<template>
	<view>
		<view class="bg-white px-3">
			<view class="py-4 line-h border-bottom d-flex a-center j-sb">
				<view class="font-30 color2">姓名</view>
				<view class="font-26 color9 text-right"><input type="text" placeholder="请输入" /></view>
			</view>
			<view class="py-4 line-h border-bottom d-flex a-center j-sb">
				<view class="font-30 color2">年龄</view>
				<view class="font-26 color9 text-right"><input type="text" placeholder="请输入" /></view>
			</view>
			<view class="py-4 line-h border-bottom d-flex a-center j-sb">
				<view class="font-30 color2">姓名</view>
				<view class="font-26 color2 d-flex">
					<view class="d-flex mr-4 a-center">
						<image src="../../static/images/icon-02.png" class="sexIcon"></image>
						<view>男</view>
					</view>
					<view class="d-flex ml-4 a-center">
						<image src="../../static/images/icon-o3.png" class="sexIcon"></image>
						<view>女</view>
					</view>
				</view>
			</view>
			<view class="py-4 line-h border-bottom d-flex a-center j-sb">
				<view class="font-30 color2">电话</view>
				<view class="font-26 color9 text-right"><input type="text" placeholder="请输入" /></view>
			</view>
		</view>
		
		<view class="font-30 mx-3 rounded10 btn"@click="Router.navigateTo({route:{path:'/pages/VIP/VIP'}})">确定</view>
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
page{background-color: #f5f5f5;}
.sexIcon{width: 32rpx;height: 32rpx;margin-right: 10rpx;}
.btn{line-height: 80rpx;text-align: center;color: #fff;background-color: #FFB2C2;margin-top: 60rpx;}
</style>
