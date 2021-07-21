<template>
	<view class="page">		
		<view class="tips-warning">
			<u-parse :html="html"></u-parse>
		</view>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {				
				html:'',
				api_name: '商标软件',
				api_url: app.globalData.api_url,
			}
		},
		onLoad() {
			//获取当前功能信息和配置分享信息
			this.getApi(this.api_name);
		},
		methods: {
			showPic(index) {
				uni.previewImage({
					current: index,
					urls: this.pics
				});
			}
		}
	}
</script>

<style>
	/* 重写样式 */
	.soBtn {
		height: 164rpx;
	}
</style>
