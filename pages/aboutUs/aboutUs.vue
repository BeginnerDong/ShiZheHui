<template>
	<view>
		
		<view class="border-bottom position-fixed left-0 right-0 top0"></view>
		<view class="">
			<view class="mx-3">
				<view class="xqInfor">
					<view class="font-30 font-weight py-2">{{mainData.title}}</view>
					<view class="cont font-26">
						<view class="content ql-editor" style="padding:0;"
						v-html="mainData.content">
						</view>
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
				mainData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['关于我们']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					};
					console.log(self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>


<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/detail.css";
	page{padding-bottom: 60rpx;}
	
	
</style>
