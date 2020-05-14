<template>
	<view>
		
		<view class="bg-white boxShaow overflow-h mt-3 mb-3">
			
			<view class="px-3 py-3">
				<view class="tabBtn d-flex a-center color6 font-26">
					<view class="tt pb-2 mr-5" :class="curr==1?'on color2 font-weight position-relative':''" @click="currChange(1)">快递包邮</view>
					<view class="tt pb-2" :class="curr==2?'on color2 font-weight position-relative':''" @click="currChange(2)">到店自提</view>
				</view>
				<view v-show="curr==1">
					<view class="d-flex j-center pt-3 pb-4" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="font-28 color6 d-flex">
							<view style="width: 40rpx;height:40rpx;margin-right: 10rpx;"><image src="../../static/images/indianal-icon1.png" mode=""></image></view>
							<view class="font-30">选择收货地址</view>
						</view>
					</view>
					<view class="d-flex j-sb a-center"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
						<view class="font-28">
							<view class="d-flex mt-1">
								<view class="mr-3 color2">张思德</view>
								<view class="color6">15689562352</view>
							</view>
							<view class="mt-2">陕西省西安市雁塔区高新大都荟</view>
						</view>
						<view class="d-flex j-end" style="width: 20%;">
							<image class="arrowR" src="../../static/images/detailsl-icon1.png" mode=""></image>
						</view>
					</view>
				</view>
				
				<view class="pt-3" v-show="curr==2">
					<view class="d-flex j-sb a-center" @click="Router.navigateTo({route:{path:'/pages/zitiAddress/zitiAddress'}})">
						<view style="width: 85%;">
							<view class="d-flex a-center">
								<view class="mr-3 font-30">门店名称门店名称</view>
								<view class="font-26">15689562352</view>
							</view>
							<view class="mt-1">陕西省西安市雁塔区高新大都荟</view>
						</view>
						<view class="arrowR"><image src="../../static/images/home-icon7.png" mode=""></image></view>
					</view>
					<view class="d-flex a-center j-sb pt-3">
						<view class="editMsg rounded10"><input type="text" value="" placeholder="输入姓名" placeholder-class="placeholder" /></view>
						<view class="editMsg rounded10"><input type="number" maxlength="11" value="" placeholder="输入电话" placeholder-class="placeholder" /></view>
					</view>
				</view>
			</view>
		</view>
		<view class="proRow mt-3">
			<view class="item d-flex j-sb mb-3" >
				<view class="pic"><image src="../../static/images/shopping-icon4.png" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow">墨西哥牛油果8枚单果200g左右</view>
					<view class="d-flex font-24 color6 mt">
						<view class="specsBtn mr-1">精装品5斤</view>
					</view>
					<view class="B-price">
						<view class="d-flex j-end">
							<view class="numBox d-flex">
								<view class="btn" @click="counter('-')">-</view>
								<view class="num">{{count}}</view>
								<view class="btn pubBj white add" @click="counter('+')">+</view>
							</view>
						</view>
						
						<view class="d-flex  a-center">
							<view class="price font-30 font-weight mr-2">88</view>
							<view class="d-flex a-center">
								<view class="font-24">会员</view>
								<view class="VipPrice font-30"><image class="arrow" src="../../static/images/home-icon6.png" mode=""></image>￥69</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar pl-3">
			<view class="d-flex a-center mr-3 font-26">总计<view class="price font-weight font-30">79</view></view>
			<view class="payBtn main-bg-color font-30 text-white">立即支付</view>
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
				curr:1,
				count:1,
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			currChange(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			}
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/proRow.css";
	page{background: #F5F5F5;padding-bottom: 140rpx;box-sizing: border-box;}
	
	/* .tabBtn .tt.on{color2: #222;font-weight: bold;} */
	.tabBtn .tt.on::before{content: "";width: 0;height: 0;border-right:14rpx solid transparent;border-left:14rpx solid transparent;border-top:14rpx solid #ffb2c2;position: absolute;left: 50%;bottom: 0;transform: translateX(-50%);}
	
	.proRow .item .pic{width: 180rpx;height: 180rpx;}
	.proRow .item .infor{width: 70%;height: 180rpx;}
	
	.editMsg{width: 48%;height: 60rpx;padding: 0 10rpx;box-sizing: border-box;border: 1px solid #eee;box-sizing: border-box;text-align: center;}
	.editMsg input{width: 100%;height: 60rpx;line-height: 60rpx;box-sizing: border-box;text-align: center;font-size: 26rpx;}
	
</style>
