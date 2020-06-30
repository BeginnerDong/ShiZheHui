<template>
	<view>
		<view class="bg-white mx-3 my-2 rounded10 font-26 color2 p-3">
			<view>物流公司：顺丰</view>
			<view class="pt-3 line-h">物流单号：SF265985625465</view>
			<view class="pt-3">包裹啥时候能到呀包裹啥时候能到呀</view>
		</view>
		
		<view class="bg-white rounded pt-5 pb px-3 mb-5 mx-3">
			<view class="d-flex a-center j-sb">
				<view class="time"></view>
				<view class="font-26 shou text-center rounded-circle mr-2">收</view>
				<view class="font-26 color2 gps avoidOverflow2">陕西省西安市雁塔区科技路高新大都荟A座2209</view>
			</view>
			<view class="d-flex a-center j-sb line-h my-5">
				<view class="time text-center">
					<view class="font-24 color2 pb-1">04.16</view>
					<view class="font-24 color6">11:20</view>
				</view>
				<view class="point rounded-circle mr-2 position-relative"></view>
				<view class="font-26 color6 gps avoidOverflow">快件到达【西安分拨中心】</view>
			</view>
			<view class="d-flex a-center j-sb line-h my-5">
				<view class="time text-center">
					<view class="font-24 color2 pb-1">04.16</view>
					<view class="font-24 color6">11:20</view>
				</view>
				<view class="point rounded-circle mr-2 position-relative"></view>
				<view class="font-26 color6 gps avoidOverflow">快件到达【西安分拨中心】</view>
			</view>
			<view class="d-flex a-center j-sb line-h my-5">
				<view class="time text-center">
					<view class="font-24 color2 pb-1">04.16</view>
					<view class="font-24 color6">11:20</view>
				</view>
				<view class="point rounded-circle mr-2 position-relative"></view>
				<view class="font-26 color6 gps avoidOverflow">快件到达【西安分拨中心】</view>
			</view>
			
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
page{background-color: #f5f5f5;}

.shou{width: 50rpx;height: 50rpx;line-height: 50rpx;background-color: #FFBDDA;color: #fff;z-index: 2;}
.time{width: 100rpx;}
.point{width: 14rpx;height: 14rpx;background-color: #dcdddc;}
.gps{width: 460rpx;}
.point::before{content: '';height: 110rpx;width: 2rpx;background-color: #e1e1e1;position: absolute;bottom: 0;left: 6rpx;}
</style>
