<template>
	<view>
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="topNavFix bg-white">
			<view class="tooling_indNav color6 mt-3 mb-2 mx-4">
				<view class="list rounded50 d-flex text-center j-sb f5bj">
					<view class="tt" :class="currNav==1?'on':''" @click="currNavChange('1')">快递包邮</view>
					<view class="tt" :class="currNav==2?'on':''" @click="currNavChange('2')">到店自提</view>
				</view>
			</view>
			<view class="orderNav d-flex j-sb a-center shadow color6" v-show="currNav==1">
				<view class="tt" :class="stateOne==1?'on':''" @click="changeStateOne('1')">全部</view>
				<view class="tt" :class="stateOne==2?'on':''" @click="changeStateOne('2')">待发货</view>
				<view class="tt" :class="stateOne==3?'on':''" @click="changeStateOne('3')">配送中</view>
				<view class="tt" :class="stateOne==4?'on':''" @click="changeStateOne('4')">已收货</view>
				<view class="tt" :class="stateOne==5?'on':''" @click="changeStateOne('5')">已评论</view>
			</view>
			<view class="orderNav d-flex j-sb a-center shadow color6" v-show="currNav==2">
				<view class="tt" :class="stateTwo==1?'on':''" @click="changeStateTwo('1')">全部</view>
				<view class="tt" :class="stateTwo==2?'on':''" @click="changeStateTwo('2')">待取货</view>
				<view class="tt" :class="stateTwo==3?'on':''" @click="changeStateTwo('3')">已核销</view>
				<view class="tt" :class="stateTwo==4?'on':''" @click="changeStateTwo('4')">已评论</view>
			</view>
		</view>
		
		<view class="topNavH"></view>
		
		<view class="mt-3 mx-3">
			<view class="proRow"  v-show="currNav==1">
				<view class="item mb-3 bg-white" v-for="(item,index) in mainData" :key="index">
					<view class="priList">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.transport_status==0&&item.isremark==0">待发货</view>
							<view class="red" v-if="item.transport_status==1&&item.isremark==0">配送中</view>
							<view class="red" v-if="item.transport_status==2&&item.isremark==0">已收货</view>
							<view class="red" v-if="item.isremark==1">已评价</view>
						</view>
						<view class="d-flex a-center j-sb mb-2"  v-for="(c_item,c_index) in item.child" :key="c_index">
							<view class="pic" v-if="item.type==1">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
								c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="pic" v-if="item.type==6">
								<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product&&
								c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow"  v-if="item.type==1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product?c_item.orderItem[0].snap_product.product.title:''}}</view>
								<view class="tit avoidOverflow"  v-if="item.type==6">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								<view class="d-flex font-24 color6 mt-1">
									<view class="specsBtn mr-1"  v-if="item.type==1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
								&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
								</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="d-flex a-center">
										<view class="price font-30 font-weight mr-2">{{c_item.unit_price?c_item.unit_price:''}}</view>
										<!-- <view class="font-24">会员</view>
										<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view> -->
									</view>
									<view class="font-26">×{{c_item.count?c_item.count:''}}</view>
								</view>
							</view>
						</view>
					</view>
					<view class="d-flex j-end">
						<view class="underBtn d-flex j-end a-center pb-3 ml-3" v-if="item.transport_status==0">
							<view class="Bbtn red" :data-id="item.id" 
							@click="Router.navigateTo({route:{path:'/pages/refund-application/refund-application?id='+$event.currentTarget.dataset.id}})">申请退款</view>
						</view>
						<view class="underBtn d-flex j-end a-center pb-3 ml-3" v-if="item.pay_status==1&&item.transport_status==1"
						@click="orderUpdate(index)">
							<view class="Bbtn red">确认收货</view>
						</view>
						<view class="underBtn d-flex j-end a-center pb-3"  v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==0&&item.type==1">
							<view class="Bbtn red" 
							:data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaList/userOrder-pingjiaList?id='+$event.currentTarget.dataset.id}})">去评价</view>
						</view>
						<view class="underBtn d-flex j-end a-center pb-3"  v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==0&&item.type==6">
							<view class="Bbtn red" 
							:data-id="item.child[0].id" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjia/userOrder-pingjia?id='+$event.currentTarget.dataset.id}})">去评价</view>
						</view>
						
						
						<view class="underBtn d-flex j-end a-center pb-3" >
							<view class="Bbtn red" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaok/userOrder-pingjiaok'}})">查看详情</view>
						</view>
						
						
						<view class="underBtn d-flex j-end a-center pb-3"  v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==1">
							<view class="Bbtn red" :data-no="item.order_no"
						 @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaok/userOrder-pingjiaok?no='+$event.currentTarget.dataset.no}})">查看评论</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="proRow"  v-show="currNav==2">
				<view class="item mb-3 bg-white"  v-for="(item,index) in mainData" :key="index">
					<view class="priList">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.transport_status==0&&item.isremark==0">待取货</view>
							<view class="red" v-if="item.transport_status==2&&item.isremark==0">已核销</view>
							<view class="red" v-if="item.isremark==1">已评价</view>
						</view>
						<view class="mb-2" v-for="(c_item,c_index) in item.child" :key="index">
							<view class="d-flex a-center j-sb">
								<view class="pic" v-if="item.type==1">
									<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product&&
									c_item.orderItem[0].snap_product.product.mainImg&&c_item.orderItem[0].snap_product.product.mainImg[0]?c_item.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
								</view>
								<view class="pic" v-if="item.type==6">
									<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product&&
									c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
								</view>
								<view class="infor">
									<view class="tit avoidOverflow"  v-if="item.type==1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
									&&c_item.orderItem[0].snap_product&&c_item.orderItem[0].snap_product.product?c_item.orderItem[0].snap_product.product.title:''}}</view>
									<view class="tit avoidOverflow"  v-if="item.type==6">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
									&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
									<view class="d-flex font-24 color6 mt-1">
										<view class="specsBtn mr-1"  v-if="item.type==1">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
									&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}</view>
									</view>
									<view class="B-price d-flex a-center j-sb">
										<view class="d-flex a-center">
											<view class="price font-30 font-weight mr-2">{{c_item.unit_price?c_item.unit_price:''}}</view>
											<!-- <view class="font-24">会员</view>
											<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view> -->
										</view>
										<view class="font-26">×{{c_item.count?c_item.count:''}}</view>
									</view>
								</view>
							</view>
							<view class="pt-3 d-flex a-center j-sb font-26" v-if="c_item.pay_status==1&&c_item.transport_status==0">
								<view>核销码：</view>
								<view class="hxEwm"  @click="hxEwmShow(index,c_index)"><image :src="c_item.qrcode" mode=""></image></view>
							</view>
						</view>
						
					</view>
					
					<view class="underBtn d-flex j-end a-center py-3"  v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==0">
						<view class="Bbtn red" 
						:data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaList/userOrder-pingjiaList?id='+$event.currentTarget.dataset.id}})">去评价</view>
					</view>
					<view class="underBtn d-flex j-end a-center py-3"  v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==1">
						<view class="Bbtn red" :data-no="item.order_no"
						 @click="Router.navigateTo({route:{path:'/pages/userOrder-pingjiaok/userOrder-pingjiaok?no='+$event.currentTarget.dataset.no}})">查看评论</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 无数据 -->
		<view class="nodata" v-if="mainData.length==0"><image src="../../static/images/nodata.png" mode=""></image></view>
		
		<!-- 核销码放大弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="hxEwmShow" v-show="is_hxEwmShow">
			<view><image class="ewm" :src="mainData[chooseIndex].child[c_chooseIndex].qrcode" mode=""></image></view>
			<view class="closeBtn" @click="hxEwmShow">×</view>
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
				wx_info:{},
				is_show:false,
				stateOne:1,
				stateTwo:1,
				orderData:2,
				currNav:1,
				is_hxEwmShow:false,
				searchItem:{
					pay_status:1,
					transport_type:1,
					level:1,
					order_step:0
				},
				mainData:[],
				Utils:this.$Utils,
				chooseIndex:-1,
				c_chooseIndex:-1,
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			//self.$Utils.loadAll(['getMainData'], self);
			if(options.num){
				self.num = options.num
			}
		},
		
		onShow() {
			const self = this;
			if(self.num){
				console.log(222)
				self.changeStateOne(self.num)
			}else{
				self.getMainData(true)
			}
			
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
			
			orderUpdate(index) {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					transport_status:2,
				};
				postData.searchItem = {
					id:self.mainData[index].id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			currNavChange(currNav){
				const self = this;
				if(currNav!= self.currNav){
					self.currNav = currNav
					delete self.searchItem.transport_status;
					delete self.searchItem.isremark;
					self.stateOne=1;
					self.stateTwo=1;
					self.searchItem.transport_type = self.currNav
					self.getMainData(true)
				}
			},
			
			changeStateOne(stateOne){
				const self = this;
				if(stateOne!= self.stateOne){
					self.stateOne = stateOne
					if(self.stateOne==1){
						delete self.searchItem.transport_status;
						delete self.searchItem.isremark;
					}else if(self.stateOne==2){
						self.searchItem.transport_status=0;
						self.searchItem.isremark=0
					}else if(self.stateOne==3){
						self.searchItem.transport_status=1;
						self.searchItem.isremark=0
					}else if(self.stateOne==4){
						self.searchItem.transport_status=2;
						self.searchItem.isremark=0
					}else if(self.stateOne==5){
						self.searchItem.transport_status=2;
						self.searchItem.isremark=1
					};
					self.getMainData(true)
				}
			},
			
			changeStateTwo(stateTwo){
				const self = this;
				if(stateTwo!= self.stateTwo){
					self.stateTwo = stateTwo
					if(self.stateTwo==1){
						delete self.searchItem.transport_status;
						delete self.searchItem.isremark;
					}else if(self.stateTwo==2){
						self.searchItem.transport_status=0;
						self.searchItem.isremark=0
					}else if(self.stateTwo==3){
						self.searchItem.transport_status=2;
						self.searchItem.isremark=0
					}else if(self.stateTwo==4){
						self.searchItem.transport_status=2;
						self.searchItem.isremark=1
					};
					self.getMainData(true)
				}
			},
			
			hxEwmShow(index,c_index){
				const self = this;
				
				if(index||index==0){
					self.chooseIndex = index
					self.c_chooseIndex = c_index
				};
				self.is_show = !self.is_show
				self.is_hxEwmShow = !self.is_hxEwmShow
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no;
	
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/proRow.css";
	/* @import "../../assets/style/editInfor.css"; */
	page{background: #F5F5F5;}
	
	.topNavFix{width: 100%;height:200rpx;position: fixed;top: 0rpx;right: 0;left: 0;box-sizing: border-box;z-index: 22;}
	.topNavH{height:200rpx;}
	
	.proRow .item{padding: 0 30rpx;}
	.proRow .item .priList{padding: 30rpx 0;border-top: 1px solid #eee;}
	.proRow .item .priList:first-child{border-top: 0;}	
	.proRow .item .pic{width: 160rpx;height: 160rpx;}
	.proRow .item .infor{width: 72%;height: 160rpx;}
	.orderNav .tt.on::after{bottom: 0;}
	
	.hxEwm{width: 80rpx;height: 80rpx;}
	
	.hxEwmShow{width: 70%;position: fixed;top: 45%;left: 50%;transform: translate(-50%,-50%);z-index: 50;}
	.hxEwmShow .ewm{width: 400rpx;height: 400rpx;display: block; margin: 0 auto;}
</style>
