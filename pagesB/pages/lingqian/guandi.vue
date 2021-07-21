<template>
	<view class="page bg-white">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="12">
					<view class="soInput">
						<view class="soInputLabel">模式：</view>
						<view class="soInputValue">
							<u-input v-model="typeName" placeholder="请选择古诗的朝代" type="select" :border="true"
								@click="showType = true" />
							<u-select v-model="showType" :list="typeList" @confirm="changeType"></u-select>
						</view>
					</view>
				</u-col>
			</u-row>
		</view>
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
				<view class="qianTitle u-text-center">{{list.qian}}签</view>
				<view class="qianTitle l-b-t-primary">签文</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qian_wen)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">解签</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.jie_qian)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">故事</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.gu_shi)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">占验</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.zhan_yan)"></u-parse>
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
				api_name: '关帝灵签',
				api_url: app.globalData.api_url,
				disabled: false,
				showWin: false,
				qiantong: app.globalData.api_url + '/uploads/xcx/lingqian/qiantong.png',
				list: [],
				typeName: '传统版解签',
				typeValue: 0,
				typeList: [{
						label: '传统版解签',
						value: 0
					},
					{
						label: '现代白话文解签',
						value: 1
					}
				],
				showType: false,
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
					that.post('ling_qian/guan_di', {
						query: that.typeValue
					}, res => {
						that.disabled = false;
						that.showWin = true;
						that.qiantong = app.globalData.api_url + '/uploads/xcx/lingqian/qiantong.png';
						that.list = res;
					});
				}, 2000);
			},
			changeType(e) {
				//选择模式
				this.typeName = e[0].label;
				this.typeValue = e[0].value;
			}
		}
	}
</script>

<style>
	/* 重写样式 */
	.soInputLabel {
		flex: 2;
		font-weight: 900;
	}

	.soInputValue {
		flex: 10;
	}
</style>
