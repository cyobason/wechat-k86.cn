<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">输入三个汉字：</view>
						<view class="soInputValue">
							<u-input maxlength="3" :border="true" v-model="key" placeholder="请随意输入3个汉字"></u-input>
						</view>
					</view>
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>
				</u-col>
			</u-row>
		</view>
		<u-popup v-model="showWin" v-if="Object.keys(list).length > 0" border-radius="20" mode="center" width="95%"
			:closeable="true">
			<view class="qianBox">
				<view class="qianTitle u-text-center">{{list.qian}} {{list.ji_xiong}}</view>
				<view class="qianTitle u-text-center">{{list.qian_ming}}</view>
				<view class="qianTitle l-b-t-primary">签文</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qian_wen)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">解签</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.jie_qian)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">详解</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.xiang_jie)"></u-parse>
				</view>
			</view>
		</u-popup>
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
				api_name: '诸葛神算',
				api_url: app.globalData.api_url,
				key: '',
				showWin: false,
				list: []
			}
		},
		onLoad() {
			//获取当前功能信息和配置分享信息
			this.getApi(this.api_name);
		},
		methods: {
			getList() {
				let that = this;
				if (!that.key) {
					that.showMsg('请随意输入3个汉字');
					return false;
				}
				//开始查询
				that.post('/ling_qian/zhu_ge', {
					words: that.key
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			}
		}
	}
</script>

<style>
	/* 重写样式 */
	.soBtn {
		height: 74rpx;
	}

	.soInputLabel {
		flex: 3;
		font-weight: 900;
	}

	.soInputValue {
		flex: 5;
	}
</style>
