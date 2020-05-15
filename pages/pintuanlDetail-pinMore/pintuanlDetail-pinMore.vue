<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="detail_join px-3 py-3">
			<view class="twolist">
				<view class="item d-flex a-center j-sb mb-3" v-for="(item,index) in pinTuanData" :key="index">
					<view class="ll d-flex a-center">
						<view class="photo">
							<image src="../../static/images/details-img2.png" mode=""></image>
						</view>
						<view class="tit color6 font-24 avoidOverflow">名称名称名称</view>
					</view>
					<view class="rr d-flex a-center j-sb">
						<view class="downTime text-center pr-1">
							<view class="font-24">还差<span class="red">1人</span>拼成</view>
							<view class="font-20 pt color9">剩余23:23:48</view>
						</view>
						<view class="go font-24 main-bg-color rounded10 text-center text-white"  @click="Router.navigateTo({route:{path:'/pages/pintuanlDetail/pintuanlDetail'}})">去拼团</view>
					</view>
				</view>
				
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				pinTuanData:5
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	@import "../../assets/style/detail.css";
	
</style>
