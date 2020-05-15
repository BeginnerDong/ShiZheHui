<template>
	<view>
		
		<view class="detailBanner position-relative">
			<swiper class="swiper-box rounded10 overflow-h" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-color="#adadad" indicator-active-color="#ffbcd7">
				<block v-for="(item,index) in labelData" :key="index">
					<swiper-item class="swiper-item">
						<image :src="item" class="slide-image" />
					</swiper-item>
				</block>
			</swiper>
		</view>
		<view class="pl-3 py-2 main-bg-color d-flex a-center j-sb detail-priceBox text-white">
			<view style="width: 85%;">
				<view class="d-flex a-center font-24">
					<view>秒杀价<span class="price font-weight">79</span></view>
					<view class="ml-3 pt-1">原价<span class="font-28 font-weight">￥121</span></view>
				</view>
				<view class="d-flex a-center font-22 mt-1 pt">
					<view class="font-26">2人团</view>
					<view class="ml-3 mr-5">库存：235</view>
					<view>销量：230</view>
				</view>
			</view>
			<view class="shareBtn d-flex j-center a-center font-24"><image class="shareIcon" src="../../static/images/details-icon3.png" mode=""></image><view class="ml-1">分享</view></view>
		</view>
		<view class="mx-3 py-3">
			<view class="font-32 font-weight pb-1">形象美奇异果洁净嫩滑洁面补水保湿洗面奶护肤品水果去角质洗面奶</view>
			
			<view class="font-26 color9">新品首发金夫人了都是感觉会根据开发商的华工科技粉红色关键人火锅积分兑换</view>
		</view>
		<view class="f5Bj-H20"></view>
		
		<!-- 拼团 -->
		<view class="detail_join px-3 py-3">
			<view class="d-flex a-center j-sb fs13">
				<view>45人正在拼团，可直接参与</view>
				<view class="font-24 color9 d-flex a-center j-end" @click="Router.navigateTo({route:{path:'/pages/pintuanlDetail-pinMore/pintuanlDetail-pinMore'}})">
					<view>查看更多</view>
					<image class="arrowR" style="width: 12rpx;height: 20rpx;" src="../../static/images/home-icon7.png" mode=""></image>
				</view>
			</view>
			<view class="twolist">
				<view class="item d-flex a-center j-sb pt-3" v-for="(item,index) in pinTuanData" :key="index">
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
						<view class="go font-24 main-bg-color rounded10 text-center text-white"  @click="spaceShow">去拼团</view>
					</view>
				</view>
				
			</view>
		</view>
		
		<view class="f5Bj-H20"></view>
		<view class="mx-3 pt-3">
			<view class="d-flex a-center j-sb">
				<view class="color6">商品评论</view>
				<view class="d-flex j-end a-center font-24 color9" @click="Router.navigateTo({route:{path:'/pages/product-PingJia/product-PingJia'}})">查看全部<image class="arrowR" src="../../static/images/home-icon7.png" mode=""></image></view>
			</view>
			<view class="detail_pjLis">
				<view class="item" style="border-bottom: none;">
					<view class="d-flex j-sb a-center pb-1 font-24">
						<view class="d-flex a-center">
							<view class="photo mr-1"><image src="../../static/images/details-img2.png" mode=""></image></view>
							<view class="name color6">哆啦A梦</view>		
						</view>
						<view class="time color9">2020-03-17</view>
					</view>
					<view class="text font-26 pt-1">根据瑞灵活赶紧来都搞好了电话沟通各环节的反馈给换了多个黑客帝国</view>
				</view>
			</view>
		</view>
		<view class="f5Bj-H20"></view>
		<view class="px-3">
			<view class="py-3">
				<view class="font-30 color6 mb-3">详情介绍</view>
				<view class="xqInfor">
					<view class="cont fs14 text-center">
						<view>管理客服电话</view>
						<view>悲愤交加鹤骨鸡肤供货</view>
						<view>方点击可供货方都拉黑干活的放假</view>
						<view>价格过节费考虑到加工费</view>
						<view><image src="../../static/images/details-img1.png" mode="widthFix"></image></view>
						<view>管理客服电话</view>
						<view>悲愤交加鹤骨鸡肤供货</view>
						<view>方点击可供货方都拉黑干活的放假</view>
						<view>价格过节费考虑到加工费</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<view class="xqbotomBar center pl-3">
			<view class="d-flex font-24">
				<view class="ite flexColumn" @click="navigateBack">
					<view class="icon"><image src="../../static/images/details-icon4.png" mode=""></image></view>
					<view class="mt-1">返回</view>
				</view>
			</view>
			<view class="d-flex text-white font-30 text-center">
				<view class="payBtn main-bgTwo" style="width: 280rpx;" @click="spaceShow">单独购买</view>
				<view class="payBtn main-bg-color" style="width: 280rpx;" @click="spaceShow">拼团购买</view>
			</view>
		</view>
		
		<!-- 规格选择 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="spaceShow bg-white" v-show="is_spaceShow">
			<view class="closebtn" @click="spaceShow">×</view>
			<view class="d-flex">
				<view class="pic rounded10 overflow-h mr-3"><image src="../../static/images/details-img3.png" mode=""></image></view>
				<view class="infor">
					<view class="red d-flex a-center font-24 mt-5 pt-2 mb-3">
						<view>秒杀价<span class="price font-weight font-44">79</span></view>
						<view class="ml-3 pt-1">原价<span class="font-28">￥121</span></view>
					</view>
					<view class="font-26">请选择规格</view>
				</view>
			</view>
			<view class="mt-3">
				<view class="font-26">规格</view>
				<view class="specsLable d-flex font-26 color6">
					<view class="tt" :class="specsCurr==index?'on':''" v-for="(item,index) in seltSpecsData" :key="index" @click="specsChange(index)">{{item}}</view>
				</view>
			</view>
			<view class="xqbotomBar px-3 mb-3" style="box-shadow:initial">
				<view class="bottom-btnCont d-flex rounded50 overflow-h text-white font-30" style="width: 100%">
					<view class="w-100 text-center main-bg-color" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})">立即购买</view>
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
				labelData: [
					"../../static/images/details-img.png",
					"../../static/images/details-img.png",
					"../../static/images/details-img.png"
				],
				pinTuanData:2,
				specsCurr:0,
				is_spaceShow:false,
				seltSpecsData:['水动力三部曲套装','水动力三部曲套装','定制版','定制版100ML','定制版120ML'],
				curr:1,
				messageData:4,
				
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			currChange(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			navigateBack(){
				uni.navigateBack({
				});
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
	@import "../../assets/style/productList.css";
	page{padding-bottom:140rpx;}
	
	.swiper-box {height: 420rpx;box-sizing: border-box;}
	.swiper-box swiper-item{width: 100%;box-sizing: border-box;overflow: hidden;}
	
	.bottom-btnCont{width: 520rpx;line-height: 80rpx;}
	.bottom-btnCont .hei{background-color: #3c3c3c;}
	
	.specsLable{flex-wrap: wrap;}
	.specsLable .tt{margin: 30rpx 50rpx 0 0;border: 1px solid #ddd;line-height: 60rpx;padding: 0 16rpx;border-radius:10rpx;}
	
	.specsLable .tt.on{background-color: #fff2f5;color: #ffb2c2;border: 1px solid #ffb2c2;}
	
	.spaceShow{width: 100%;border-radius: 20rpx 20rpx 0 0;position: fixed;left: 0;right: 0;bottom: 0;height: 800rpx;z-index: 50;padding: 30rpx;padding-bottom: 140rpx;}
	.spaceShow .pic{width: 210rpx;height: 210rpx;}
	
	.main-bgTwo{background-color: #ffcdd7;}
</style>
