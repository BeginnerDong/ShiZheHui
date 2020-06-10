<template>
	<view>
		
		<view class="detailBanner">
			<swiper class="swiper-box rounded10 overflow-h" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-color="#adadad" indicator-active-color="#ffbcd7">
				<block v-for="(item,index) in mainData.bannerImg" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item.url" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="pl-3 py-2 main-bg-color d-flex a-center j-sb detail-priceBox text-white">
			<view style="width: 85%;">
				<view class="d-flex a-center">
					<view class="price font-30 font-weight mr-2">{{mainData.sku[specsCurr]?mainData.sku[specsCurr].price:''}}</view>
					<view class="font-24">会员</view>
					<view class="VipPrice font-30 font-weight"><image class="arrow" src="../../static/images/details-icon.png" mode="">
					</image>￥{{mainData.sku[specsCurr]?mainData.sku[specsCurr].member_price:''}}</view>
				</view>
				<view class="d-flex a-center font-22 mt-1">
					<view>库存：{{mainData.sku[specsCurr]?mainData.sku[specsCurr].stock:'0'}}</view>
					<view class="ml-5">销量：{{mainData.sku[specsCurr]?mainData.sku[specsCurr].sale_count:''}}</view>
				</view>
			</view>
			<button open-type="share" class="shareBtn d-flex j-center a-center font-24">
				<image class="shareIcon" src="../../static/images/details-icon3.png" mode=""></image><view class="ml-1">分享</view></button>
		</view>
		<view class="mx-3 py-3">
			<view class="font-32 font-weight pb-2">{{mainData.title}}</view>
			<view class="font-26 color9">{{mainData.description}}</view>
		</view>
		
		<view class="f5Bj-H20"></view>
		<view class="mx-3 py-3">
			<view class="d-flex j-sb a-center" @click="spaceShow">
				<view class="d-flex a-center">
					<view class="font-26 mr-3 color6">规格选择</view>
					<view class="d-flex a-center flex-wrap" style="width: 466rpx;"><view class="mr-1">已选：{{mainData.sku[specsCurr]?mainData.sku[specsCurr].title:''}}</view></view>
				</view>
				<view style="width: 44rpx;height: 8rpx;"><image src="../../static/images/details-icon1.png" mode=""></image></view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		<view class="px-3">
			<view class="tooling_indNav color6 my-3 mx-4">
				<view class="list rounded50 d-flex text-center j-sb f5bj ">
					<view class="tt" :class="curr==1?'on':''" @click="currChange('1')">详情介绍</view>
					<view class="tt" :class="curr==2?'on':''" @click="currChange('2')">商品评论</view>
				</view>
			</view>
			<view class="py-3 w-100">
				<view class=" xqInfor" v-show="curr==1">
					<view class="cont fs14 text-center">
						<view class="content ql-editor" style="padding:0;" v-html="mainData.content">
						</view>
					</view>
				</view>
				<!-- 评论 -->
				<view v-show="curr==2">
					<view class="detail_pjLis" v-if="messageData.length>0">
						<view class="item" v-for="(item,index) in messageData" :key="index">
							<view class="d-flex j-sb a-center pb-1 font-24">
								<view class="d-flex a-center">
									<view class="photo mr-1"><image :src="item.headImg&&item.headImg[0]&&item.headImg[0].url!=''?item.headImg[0].url:''" mode=""></image></view>
									<view class="name color6">{{item.title!=''?item.title:'用户'}}</view>		
								</view>
								<view class="time color9">{{item.create_time}}</view>
							</view>
							<view class="text font-26 pt-1">{{item.description}}</view>
							<view class="picLis d-flex a-start flex-wrap" >
								<view class="pic" v-for="(c_item,c_index) in item.mainImg">
									<image :src="c_item.url" mode=""></image>
								</view>
							</view>
						</view>
					</view>
					<view v-else style="width: 100%;text-align: center;">暂无评论</view>
				</view>
			</view>
			
		</view>
		
		<view class="xqbotomBar center pl-3">
			<view class="d-flex fs12">
				<view class="ite flexColumn" @click="Router.back(1)">
					<view class="icon"><image src="../../static/images/details-icon4.png" mode=""></image></view>
					<view class="mt-1">返回</view>
				</view>
			</view>
			<view class=" d-flex text-white font-30 text-center">
				<view class="payBtn main-bgTwo" style="width: 280rpx;" @click="spaceShow">加入购物车</view>
				<view class="payBtn main-bg-color" style="width: 280rpx;" @click="spaceShow">立即购买</view>
			</view>
		</view>
		
		<!-- 规格选择 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="spaceShow bg-white" v-show="is_spaceShow">
			<view class="closebtn" @click="spaceShow">×</view>
			<view class="d-flex">
				<view class="pic rounded10 overflow-h mr-3"><image :src="mainData.sku[specsCurr]&&mainData.sku[specsCurr].mainImg&&mainData.sku[specsCurr].mainImg[0]
					?mainData.sku[specsCurr].mainImg[0].url:''" mode=""></image></view>
				<view class="infor">
					<view class="d-flex a-center mt-5 pt-2 mb-3">
						<view class="price font-40 font-weight mr-2">{{mainData.sku[specsCurr]?mainData.sku[specsCurr].price:''}}</view>
						<view class="font-24">会员</view>
						<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥{{mainData.sku[specsCurr]?mainData.sku[specsCurr].member_price:''}}</view>
					</view>
					<view class="font-26">库存：{{mainData.sku[specsCurr]?mainData.sku[specsCurr].stock:'0'}}</view>
				</view>
			</view>
			<view class="mt-3">
				<!-- <view class="font-26">规格</view> -->
				<!-- <view class="specsLable d-flex font-26 color6">
					<view class="tt" :class="specsCurr==index?'on':''" 
					v-for="(item,index) in mainData.sku" :key="index" @click="specsChange(index)">{{item.title}}</view>
				</view> -->
				<scroll-view class="specsLable d-flex font-26 color6" scroll-y="true" style="height: 360rpx;">
					<view v-for="(item,index) in labelData" :key="index" style="margin-top: 20rpx;">
						<view class="fs13">{{item.title}}</view>
						<view class="tt" v-for="(c_item,c_index) in item.children" 
						style="display: inline-block;"
						 :class="Utils.inArray(c_item.id,choose_sku_item)==-1?'cantChoose'
						 :(Utils.inArray(c_item.id,sku_item)!=-1?'on':'')"
						  :key="c_index" :data-c_id="c_item.id" :data-id="item.id"
						@click="Utils.inArray($event.currentTarget.dataset.c_id,choose_sku_item)!=-1?chooseSku($event.currentTarget.dataset.id,$event.currentTarget.dataset.c_id):''">{{c_item.title}}</view>
					</view>
				</scroll-view>
			</view>
			<view class="xqbotomBar px-3 mb-3" style="box-shadow:initial">
				<view class="bottom-btnCont d-flex rounded50 overflow-h text-white font-30" style="width: 100%">
					<view class="w-50 text-center main-bgTwo mr" @click="addCar">加入购物车</view>
					<view class="w-50 text-center main-bg-color ml" 
					@click="goBuy">立即购买</view>
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
				Utils:this.$Utils,
				showView: false,
				wx_info:{},
				is_show:false,
				
				specsCurr:0,
				is_spaceShow:false,
				seltSpecsData:['水动力三部曲套装','水动力三部曲套装','定制版','定制版100ML','定制版120ML'],
				curr:1,
				
				mainData:{},
				messageData:[],
				orderList:[],
				labelData:[],
				skuData:[],
				
				sku_item:[],
				choose_sku_item:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.from === 'button') {
				return {
					title: self.mainData.title,
					path: '/pages/productDetail/productDetail?id=' + self.mainData.id, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData.mainImg[0].url ? self.mainData.mainImg[0].url : '',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			} else {
				return {
					title: self.mainData.title,
					path: '/pages/productDetail/productDetail?id=' + self.mainData.id, //点击分享的图片进到哪一个页面
					imageUrl: self.mainData.mainImg[0].url ? self.mainData.mainImg[0].url : '',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}
		},
		
		methods: {
			
			chooseSku(parentid,id){
				const self = this;
			    self.skuData = {};
			    if(self.choose_sku_item.indexOf(id)==-1){
			      return;
			    };
			    self.choose_sku_item = [];
			    var sku = self.mainData.label[parentid];
			    for(var i=0;i<sku.children.length;i++){
			      if(self.sku_item.indexOf(sku.children[i].id)!=-1){
			        self.sku_item.splice(self.sku_item.indexOf(sku.children[i].id), 1);
			      };
			      self.choose_sku_item.push(sku.children[i].id);
			    };
			
			    
			    for (var i = 0; i < self.mainData.sku.length; i++) {
			      if(self.mainData.sku[i].sku_item.indexOf(parseInt(id))!=-1){
			        self.choose_sku_item.push.apply(self.choose_sku_item,self.mainData.sku[i].sku_item);  
			      };
			    };
			
			    for(var i=0;i<self.sku_item.length;i++){
			      if(self.choose_sku_item.indexOf(parseInt(self.sku_item[i]))==-1){
			        self.sku_item.splice(i, 1); 
			      };
			    };
			    self.sku_item.push(id);
			    for(var i=0;i<self.mainData.sku.length;i++){ 
			      if(JSON.stringify(self.mainData.sku[i].sku_item.sort())==JSON.stringify(self.sku_item.sort())){
			        //self.id = self.mainData.sku[i].id;
			        self.skuData = self.$Utils.cloneForm(self.mainData.sku[i]);
					self.specsCurr = i
			      };   
			    }; 
			},
			
			currChange(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			
			specsChange(index){
				const self = this;
				self.specsCurr = index
			},
			
			spaceShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_spaceShow = !self.is_spaceShow 
			},
			
			getMainData() {
				const self = this;
				var now = new Date().getTime();
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				postData.getAfter = {
					sku: {
						tableName: 'Sku',
						middleKey: 'product_no',
						key: 'product_no',
						condition: '=',
						searchItem: {
							status: 1
						}
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						for(var key in self.mainData.label){
						  if(self.mainData.sku_array.indexOf(parseInt(key))!=-1){
						    self.labelData.push(self.mainData.label[key])
						  };    
						};
						console.log('self.labelData',self.labelData)
						for (var i = 0; i < self.mainData.sku.length; i++) {
						  /* if(self.mainData.sku[i].id==self.id){
						    self.skuData = self.$Utils.cloneForm(self.mainData.sku[i]);
						  }; */
										
						  self.choose_sku_item.push.apply(self.choose_sku_item,self.mainData.sku[i].sku_item);
						};
						self.skuData = self.$Utils.cloneForm(self.mainData.sku[0]);
						self.sku_item = self.skuData.sku_item;
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					self.getMessageData();
					self.$Utils.finishFunc('getMainData');
			
				};
				self.$apis.productGet(postData, callback);
			},
			
			getMessageData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 3
				};
				postData.searchItem = {
					thirdapp_id: 2,
					product_no: self.mainData.product_no,
					type: 1,
					user_type: 0
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData.push.apply(self.messageData, res.info.data);
					}
					console.log('self.messageData', self.messageData)
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			goBuy() {
				const self = this;
				if(self.mainData.sku[self.specsCurr].stock==0){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('库存不足！', 'none');
					return
				};
				uni.setStorageSync('canClick', false);
				self.orderList.push({
					sku_id: self.mainData.sku[self.specsCurr].id,
					count: 1,
					type: self.mainData.type,
					product: self.mainData,
					skuIndex: self.specsCurr
				}, );
				uni.setStorageSync('payPro', self.orderList);
				self.Router.navigateTo({
					route: {
						path: '/pages/orderConfim/orderConfim'
					}
				})
				uni.setStorageSync('canClick', true);
			},
			
			addCar() {
				const self = this;
				if(self.mainData.sku[self.specsCurr].stock==0){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('库存不足！', 'none');
					return
				};
				var obj = self.mainData;
				self.mainData.skuIndex = self.specsCurr;
				self.mainData.skuId = self.mainData.sku[self.specsCurr].id;
				var array = self.$Utils.getStorageArray('cartData');
				for (var i = 0; i < array.length; i++) {
					if (array[i].sku[array[i].skuIndex].id == self.mainData.sku[self.specsCurr].id) {
						var target = array[i]
					}
				}
				console.log('target',target)
				if (target&&target!=undefined) {
					target.count = target.count + 1
				} else {
					var target = self.mainData;
					target.count = 1;
					target.isSelect = true;
				}
				self.$Utils.setStorageArray('cartData', target, 'skuId', 999);
				uni.showModal({
					title: '提示',
					content: '所选规格已成功加入购物车',
					showCancel: true,
					cancelText: '取消',
					confirmText: '去购物车',
					success: res => {
						if (res.confirm) {
							self.Router.reLaunch({route:{path:'/pages/car/car'}})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					},
				});
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/detail.css";
	@import "../../assets/style/productList.css";
	page{padding-bottom:140rpx;}
	
	/* .banner-box {width: 100%;height: 420rpx;box-sizing: border-box;overflow: hidden;} */
	.swiper-box {height: 750rpx;box-sizing: border-box;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	
	.bottom-btnCont{width: 520rpx;line-height: 80rpx;}
	.bottom-btnCont .hei{background-color: #3c3c3c;}
	
	.specsLable{flex-wrap: wrap;}
	.specsLable .tt{margin: 30rpx 50rpx 0 0;border: 1px solid #ddd;line-height: 60rpx;padding: 0 16rpx;border-radius:10rpx;}
	
	.specsLable .tt.on{background-color: #fff2f5;color: #ffb2c2;border: 1px solid #ffb2c2;}
	.specsLable .tt.cantChoose{border:1px dashed;}
	.spaceShow{width: 100%;border-radius: 20rpx 20rpx 0 0;position: fixed;left: 0;right: 0;bottom: 0;height: 800rpx;z-index: 50;padding: 30rpx;padding-bottom: 140rpx;}
	.spaceShow .pic{width: 210rpx;height: 210rpx;}
	
	.main-bgTwo{background-color: #ffcdd7;}
	
	.cont .content img{width: 100%;height: auto;}
</style>
