<template>
	<view>
		
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		
		<view class="RankingTit bg-white px-3 py-2 text-center d-flex a-center j-sb">
			<view class="tt flex-1 d-flex a-center j-center">
				<view class="d-flex a-center" @click="changeOrder('price')">
					<view>价格排行</view>
					<view class="arrowTB">
						<view class="arrowB">
							<image :src="order.price&&order.price=='asc'?'../../static/images/classificationl-icon1.png':'../../static/images/classificationl-icon3.png'" mode=""></image>
						</view>
						<view class="arrowB mt">
							<image :src="order.price&&order.price=='desc'?'../../static/images/classificationl-icon4.png':'../../static/images/classificationl-icon2.png'"  mode=""></image>
						</view>
					</view>
				</view>
			</view>
			<view class="tt flex-1 d-flex a-center j-center">
				<view class="d-flex a-center"  @click="changeOrder('sale_count')">
					<view>销量排行</view>
					<view class="arrowTB">
						<view class="arrowB">
							<image :src="order.sale_count&&order.sale_count=='asc'?'../../static/images/classificationl-icon1.png':'../../static/images/classificationl-icon3.png'" mode=""></image>
						</view>
						<view class="arrowB mt">
							<image :src="order.sale_count&&order.sale_count=='desc'?'../../static/images/classificationl-icon4.png':'../../static/images/classificationl-icon2.png'" mode=""></image>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="HotProduct px-3 d-flex j-sb d-flex j-sb flex-wrap ">
			<view class="item rounded10 mt-3" v-for="(item,index) in mainData" :key="index" 
				:data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
				<view class="infor">
					<view class="tit font-30 d-flex a-center">
						<view class="avoidOverflow2">{{item.title}}</view>
					</view>
					<view class="d-flex a-center mt-2">
						<view class="price font-30 font-weight mr-2">{{item.price}}</view>
						<view class="font-24">会员</view>
						<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥{{item.member_price}}</view>
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
				},
				order:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.searchItem.category_id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			changeOrder(type){
				const self = this;
				if(self.order[type]){
					if(self.order[type]=='asc'){
						self.order[type] = 'desc'
					}else{
						self.order[type] = 'asc'
					}
				}else{
					self.order[type] = 'asc'
				};
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
				if(JSON.stringify(self.order)!='{}'){
					postData.order = self.$Utils.cloneForm(self.order)
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
	/* @import "../../assets/style/orderNav.css"; */
	@import "../../assets/style/productList.css";
	
	page{padding-bottom: 60rpx;background-color: #F5F5F5;}	
	.orderNav{position: fixed; left:0; right: 0;top: 0; z-index: 10;}
	.orderNavH{height: 80rpx;}
	.orderNav .tt{width: auto;}
	.orderNav .tt.on{color: #222;font-size: 28rpx;font-weight: bold;}
	
	.arrowB{width: 16rpx;height: 8rpx;}
</style>
