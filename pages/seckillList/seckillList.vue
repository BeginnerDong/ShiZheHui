<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<!-- 搜索 -->
		<view class="seachbox d-flex a-center j-sb bg-white px-3 py-3"> 
			<view class="d-flex a-center rr f5bj" style="width: 85%;" @click="Router.navigateTo({route:{path:'/pages/seachProduct/seachProduct'}})">
				<button class="seachBtn" type="button"><image src="../../static/images/home-icon1.png" mode=""></image></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="搜索您想要的商品" placeholder-class="placeholder" />
				</view>
				<view class="delt d-flex j-center a-center"><text>×</text></view>
			</view>
			<view class="position-relative mr-2" style="width: 40rpx;height: 40rpx;">
				<view class="carNum">3</view>
				<image src="../../static/images/seconds kill-icon.png" mode=""></image>
			</view>
		</view>
		
		<view class="mx-3 mt-3">
			<view class="proRow">
				<view class="item d-flex a-start j-sb mt-3" v-for="(item,index) in seckillData" :key="index" @click="Router.navigateTo({route:{path:'/pages/seckillDetail/seckillDetail'}})">
					<view class="pic"><image src="../../static/images/home-img01.png" mode=""></image></view>
					<view class="infor">
						<view class="avoidOverflow2 font-30">12色可水洗油画棒 水溶性旋转涂料板</view>
						<view class="B-price">
							<view class="d-flex a-center">
								<view class="red">秒杀价<span class="price">79</span></view>
								<view class="yuanJia ml-3">121</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 无数据 -->
		<view class="nodata"><image src="../../static/images/nodata.png" mode=""></image></view>
		
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
				seckillData:3
			}
		},
		
		onLoad(options) {
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
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proRow.css";
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}
	
</style>

