<template>
	<view>
		<view class="mx-3">
			<view class="myaddress-lis d-flex a-center j-sb bg-white rounded10 overflow-h mt-3" v-for="(item,index) in mainData" :key="index" >
				<view class="ll">
					<view class="d-flex a-center  title">
						<view class="logo rounded10 overflow-h mr-2"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode="">
						</image></view>
						<view class="" style="w75%">
							<view class="name d-flex a-center">{{item.name}}</view>
							<view class="mt-2 numb color6 font-26">{{item.phone}}</view>
						</view>
					</view>
					
					<view class="adrs">{{item.address}}</view>
				</view>
				<view class="rr d-flex j-end a-center">
					<view class="seltBox" @click="setDefault(index)">
						<image class="icon" :src="passage1!=''&&passage1 == item.user_no?'../../static/images/shopping-icon.png':'../../static/images/shopping-icon1.png'" mode=""></image>
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
				
				mainData:[],
				passage1:''
			}
		},

		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
		},

		
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		

		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					thirdapp_id:2
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0]
						self.passage1 = self.userInfoData.passage1
					};
					self.$Utils.finishFunc('getUserInfoData');	
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			setDefault(index) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {
					passage1:self.mainData[index].user_no
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.passage1 = self.mainData[index].user_no
						uni.setStorageSync('choosedShopData', self.mainData[index]);
						console.log('choosedIndex', self.choosedIndex);
						uni.navigateBack({
							delta: 1
						})
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					user_type:1,
					thirdapp_id:2
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
						
					};
					self.$Utils.finishFunc('getMainData');	
				};
				self.$apis.userInfoGet(postData, callback);
			},
		}
	}
</script>

<style>
	page{ background: #f5f5f5;}
	.myaddress-lis{padding:30rpx 4%;background: #fff; }
	.myaddress-lis .ll{ width: 85%;}
	.myaddress-lis .ll .logo{width: 100rpx;height: 100rpx;}
	/* .myaddress-lis .name{ font-size: 28rpx; color: #222;} */
	.myaddress-lis .ll .title{flex-wrap: wrap;}
	.myaddress-lis .ll .icon{ width: 26rpx; height: 26rpx; display: inline-block; margin-right: 10rpx;}
	.myaddress-lis .adrs{ font-size: 26rpx;line-height: 40rpx;padding-top: 16rpx;}
	
	.myaddress-lis .rr{ width: 15%;}
	.seltBox{ width:40rpx; height: 40rpx; display:block;}
</style>
