<template>
	<view>
		
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="font-26 px-3 py-3">
			<textarea value="" placeholder="您对我们平台有什么好建议吗？" />
		</view>
		<view class="f5Bj-H10"></view>
		
		<view class="submitbtn pdtb15"  style="margin-top: 200rpx;">
			<button class="btn" type="button" >提交</button>
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
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	/* @import "../../assets/style/detail.css"; */
	page{padding-bottom: 60rpx;}
	textarea{font-size: 26rpx;width: 100%;height:400rpx;padding: 0;}
	
</style>
