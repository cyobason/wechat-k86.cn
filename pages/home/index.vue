<template>
	<view class="page u-p-b-50">
		<u-swiper :list="baners" border-radius="0"></u-swiper>
		<block v-for="(v,i) in category" :key="i" v-if="list[v.id]">			
			<view class="itemTitle c-info l-b-t-primary u-m-t-20 u-m-b-20">{{v.name}}</view>
			<u-grid :col="3">
				<navigator v-for="(vv,ii) in list[v.id]" :key="ii" :url="'/' + vv.page_url" hover-class="none">
					<u-grid-item>
						<image :src="api_url + vv.picimage" mode="aspectFit" class="icon"></image>
						<view>{{vv.title}}</view>
					</u-grid-item>
				</navigator>
			</u-grid>
		</block>		
		<u-tabbar :list="tabbar" active-color="#2979ff" inactive-color="#424242"></u-tabbar>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				list: [],
				category: [],
				tabbar: this.tabbar,
				api_url: app.globalData.api_url,				
				baners: []
			}
		},
		onLoad() {
			this.getList();
		},
		onShareAppMessage() {
			//分享
			return {
				title: app.globalData.title,
				imageUrl: this.baners[0],
				path: '/pages/home/index'
			}
		},
		onShareTimeline() {
			//分享到朋友圈
			return {
				title: app.globalData.title,
				path: '/pages/home/index'
			}
		},
		methods: {
			getList() {
				let that = this;
				//开始查询
				that.post(app.globalData.api_url + '/api', {}, res => {					
					that.category = res.data.category;
					that.list = res.data.list;
					that.baners = res.data.site.xcx_baner;					
				});
			}
		}
	}
</script>

<style>
	.itemTitle {
		padding: 20rpx;
		font-size: 0.9rem;
		font-weight: 600;
	}

	.icon {
		width: 120rpx;
		height: 120rpx;
		border-radius: 20%;
		margin-bottom: 10rpx;
	}
</style>
