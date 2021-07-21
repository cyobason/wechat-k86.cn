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
				<view class="qianTitle u-text-center">{{list.qian}} {{list.qian_ming}}</view>
				<view class="qianTitle l-b-t-primary">干支</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.gan_zhi)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">签文</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qian_wen)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">解签</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.jie_qian)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">诗曰</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.shi_yue)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">米力仙注</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.mi_li_xian_zhu)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">运程</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.yun_cheng)"></u-parse>
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
				api_name: '财神灵签',
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
					that.post('ling_qian/cai_shen', {}, res => {
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
