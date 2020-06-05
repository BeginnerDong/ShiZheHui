<template>
	<view>
		<view class="mt-3 mx-3">
			<view class="proRow ">
				<view class="item mb-3 bg-white">
					<view class="priList">
						<view class="font-24 d-flex j-sb a-center mb-2">
							<view class="color9">交易时间：{{mainData.create_time?mainData.create_time:''}}</view>
							<view class="red">已收货</view>
						</view>
						<view class="d-flex a-center j-sb">
							<view class="pic" v-if="mainData.type==1">
								<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product
							&&mainData.orderItem[0].snap_product.product.mainImg&&mainData.orderItem[0].snap_product.product.mainImg[0]?mainData.orderItem[0].snap_product.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="pic" v-if="mainData.type==6">
								<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product
							&&mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="tit avoidOverflow" v-if="mainData.type==1">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product
							?mainData.orderItem[0].snap_product.product.title:''}}</view>
							<view class="tit avoidOverflow" v-if="mainData.type==6">{{mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product
							?mainData.orderItem[0].snap_product.title:''}}</view>
								<view class="d-flex font-24 color6 mt-1" v-if="mainData.type==1">
									<view class="specsBtn mr-1">{{mainData.title}}</view>
								</view>
								<view class="B-price d-flex a-center j-sb">
									<view class="d-flex a-center">
										<view class="price font-30 font-weight mr-2">{{mainData.price?mainData.price:''}}</view>
										<!-- <view class="font-24">会员</view>
										<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view> -->
									</view>
									<view class="font-26">×{{mainData.count?mainData.count:''}}</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="bg-white rounded10 px-3 py-3">
				<view class="font-26">
					<textarea v-model="submitData.description" placeholder="请填写商品评论,让更多人知道" />
				</view>
				<view class="d-flex a-center flex-wrap pjPic">
					<image v-for="item in submitData.mainImg" :src="item.url" mode=""></image>
					<image  @click="upLoadImg('mainImg')" src="../../static/images/commental-img.png" mode=""></image>
				
					<!-- <view class="font-24 color9 ml-2">可上传多张照片</view> -->
				</view>
				
				
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 120rpx;">
			<button class="btn" type="button" @click="Utils.stopMultiClick(submit)">确定</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{},
				submitData: {
					order_no: '',
					product_no: '',
					description: '',
					type:1,
					title:'',
					headImg :[],
					//passage1:'',
					mainImg:[],
					thirdapp_id:2
				},
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			var res = self.$Token.getProjectToken(function(){
				self.$Utils.loadAll(['getMainData'], self)
			});
			if(res){
				self.$Utils.loadAll(['getMainData'], self)
			};
			self.submitData.headImg  = [{type:'image',url:uni.getStorageSync('user_info').headImgUrl}];
			self.submitData.title = uni.getStorageSync('user_info').nickname
		},
		
		methods: {
			
			upLoadImg(type) {
				const self = this;	
				if (self.submitData[type].length > 8) {
					api.showToast('仅限9张', 'fail');
					return;
				};
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log('type',type)
						console.log(self.submitData)
						wx.hideLoading()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 9,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths;
						console.log(callback)
						for (var i = 0; i < tempFilePaths.length; i++) {
							var file = res.tempFiles[i];
							var obj = res.tempFiles[i].path.lastIndexOf(".");
							var ext = res.tempFiles[i].path.substr(obj+1);
							self.$Utils.uploadFile(tempFilePaths[i], 'file', {
								tokenFuncName: 'getProjectToken',ext:ext,md5:'md5',totalSize:file.size,start:0,chunkSize:file.size,originName:'img'
							}, callback)
						}
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id,
				};
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'=',
						
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.order_no = self.mainData.order_no;
						//self.submitData.passage1 = self.mainData.parent_no;
						self.submitData.product_no = self.mainData.type==1?self.mainData.orderItem[0].snap_product.product.product_no:self.mainData.orderItem[0].snap_product.product_no
					};
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.headImg ;
				delete newObject.title;
				delete newObject.mainImg;
				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					self.messageAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [{
				  tableName:'Order',
				  FuncName:'update',
				  searchItem:{
				    id:self.id
				  },
				  data:{
				    isremark:1,
				  }
				}];
				const callback = (data) => {
			
					if (data.solely_code == 100000) {
			
						self.$Utils.showToast('评价成功', 'none')
						setTimeout(function() {
							self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
			
				};
				self.$apis.messageAdd(postData, callback);
			},
			
		}
	};
</script>

<style>
	
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 60rpx;}
	textarea{font-size: 26rpx;width: 100%;height:240rpx;padding: 0;}
	.onloadbtn{width: 120rpx;height: 120rpx;}
	.pjPic image{width: 142rpx;height: 142rpx;margin: 20rpx 20rpx 0 0;border-radius: 10rpx;}
	.pjPic image:nth-of-type(4n){margin-right: 0;}
</style>
