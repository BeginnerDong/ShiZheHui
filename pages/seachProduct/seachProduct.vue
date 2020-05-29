<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox d-flex a-center j-sb bg-white border-bottom px-3 py-3"> 
			<view class="d-flex a-center rr f5bj" style="width: 85%;">
				<button class="seachBtn" type="button"><image src="../../static/images/home-icon1.png" mode=""></image></button>
				<view class="input">
					<input type="text" v-model="title" placeholder="搜索您想要的商品" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="font-30 main-text-color" @click="search">搜索</view>
		</view>
		
		<view class="mx-3 mt-3">
			<view class="proRow">
				<view class="item d-flex a-start j-sb mt-3" v-for="(item,index) in mainData" :key="index" 
				@click="Router.navigateTo({route:{path:'/pages//'}})">
					<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="avoidOverflow2 font-30">{{item.title}}</view>
						<view class="B-price">
							<view class="d-flex a-center">
								<view class="price font-30 font-weight mr-2">{{item.price}}</view>
								<view class="font-24">会员</view>
								<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥{{item.member_price}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				title:'',
				mainData:[],
				searchItem:{
					thirdapp_id:2,
					type:1
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			search(){
				const self = this;
				if(self.title!=''){
					self.searchItem.title = ['like',['%'+self.title+'%']];
				}else{
					delete self.searchItem.title
				}
				self.getMainData(true)
			},
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem)
				postData.order = {
					listorder: 'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
					
					};
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/seach.css";
	@import "../../assets/style/proRow.css";
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}
	
</style>

