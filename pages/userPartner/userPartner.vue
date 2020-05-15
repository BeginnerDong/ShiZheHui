<template>
	<view>
		
		<view>
			<image class="w-100" src="../../static/images/brand-cooperationl-img.png" mode="widthFix"></image>
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
	
</style>
