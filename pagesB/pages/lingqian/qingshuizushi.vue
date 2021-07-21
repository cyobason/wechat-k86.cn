<template>
	<view class="page bg-white">
		<view class="chouqianBox">
			<view class="chouqianImg">
				<image :src="qiantong" mode="aspectFit"></image>
			</view>
			<view class="chouqianBtn">
				<u-button type="warning" :disabled="disabled" @click="getChouQian">开始抽签</u-button>
			</view>
		</view>
		<view class="tips-warning">
			<u-parse :html="html"></u-parse>
		</view>
		<u-popup v-model="showWin" v-if="Object.keys(list).length > 0" border-radius="20" mode="center" width="95%"
			:closeable="true">
			<view class="qianBox">
				<view class="qianTitle u-text-center">{{list.qian}}</view>
				<view class="qianTitle l-b-t-primary">签文</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qian_wen)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">解签</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.jie_qian)"></u-parse>
				</view>
			</view>
		</u-popup>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				html:'',
				api_name: '清水祖师灵签',
				api_url: app.globalData.api_url,
				disabled: false,
				showWin: false,
				qiantong: app.globalData.api_url + '/uploads/xcx/lingqian/qiantong.png',
				list: []
			}
		},
		onLoad() {
			//获取当前功能信息和配置分享信息
			this.getApi(this.api_name);		
		},
		methods: {
			getChouQian() {				
				let that = this;
				that.qiantong = app.globalData.api_url + '/uploads/xcx/lingqian/qiantong.gif';
				that.disabled = true;
				//延迟2秒显示签文
				setTimeout(() => {
					//开始查询
					that.post('ling_qian/qing_shui_zu_shi', {}, res => {
						that.disabled = false;
						that.showWin = true;
						that.qiantong = app.globalData.api_url + '/uploads/xcx/lingqian/qiantong.png';
						that.list = res;
					});
				}, 2000);
			}
		}
	}
</script>

<style>
	
</style>
