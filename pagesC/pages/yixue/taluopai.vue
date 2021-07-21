<template>
	<view class="page">
		<view class="tips-warning">
			<u-parse :html="html"></u-parse>
		</view>
		<scroll-view :scroll-x="true" scroll-left="100" v-if="Object.keys(list).length <= 0">
			<view class="taluopaiBox">
				<view class="taluopaoList" v-for="i in pai" :key="i">
					<image :src="api_url + '/uploads/xcx/taluopai/taluopai_bg.jpg'" mode="aspectFit"
						@click="delPai(i)">
				</view>
			</view>
		</scroll-view>
		<view class="soList" v-for="(v,i) in list" :key="i" v-if="Object.keys(list).length > 0">
			<u-row gutter="16">
				<u-col span="4">
					<image :class="v[6] == '逆位' ? 'fanzhuan' : ''"
						:src="api_url + '/uploads/xcx/taluopai/'+v[4]+'.jpg'" mode="aspectFit"></image>
					<view class="soListTitle u-text-center">牌位{{i + 1}}</view>
					<view class="soListTitle u-text-center">{{v[0]}}</view>
					<view class="soListTitle u-text-center">{{v[1]}}</view>
					<view class="soListTitle u-text-center">【{{v[6]}}】</view>
				</u-col>
				<u-col span="8">
					<view class="soListTitle l-b-t-primary">相关语</view>
					<view class="soListContent c-info">{{v[2]}}</view>
					<view class="soListTitle l-b-t-primary">对应星象</view>
					<view class="soListContent c-info">{{v[3]}}</view>
					<view class="soListTitle l-b-t-primary">解读</view>
					<view class="soListContent c-info">{{v[5]}}</view>
					<view class="soListTitle l-b-t-primary">解释</view>
					<view class="soListContent c-info">
						<u-parse :html="nl2br(v[7])"></u-parse>
					</view>
				</u-col>
			</u-row>
		</view>
		<view class="taluopaiBtn">
			<u-button type="warning" @click="changeXiPai">重新洗牌</u-button>
		</view>
		<block v-if="Object.keys(list).length <= 0">
			<view class="taluopaiShiZi">
				<u-grid :col="3">
					<u-grid-item>
						<image class="taluopaiBG" :src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'"
							mode="aspectFit"></image>
					</u-grid-item>
					<u-grid-item>
						<!-- 3 -->
						<image v-if="index >= 3" class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_bg.jpg'" mode="aspectFit"></image>
						<image v-else class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'" mode="aspectFit">
						</image>
					</u-grid-item>
					<u-grid-item>
						<image class="taluopaiBG" :src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'"
							mode="aspectFit"></image>
					</u-grid-item>
					<u-grid-item>
						<!-- 1 -->
						<image v-if="index >= 1" class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_bg.jpg'" mode="aspectFit"></image>
						<image v-else class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'" mode="aspectFit">
						</image>
					</u-grid-item>
					<u-grid-item>
						<!-- 5 -->
						<image v-if="index >= 5" class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_bg.jpg'" mode="aspectFit"></image>
						<image v-else class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'" mode="aspectFit">
						</image>
					</u-grid-item>
					<u-grid-item>
						<!-- 2 -->
						<image v-if="index >= 2" class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_bg.jpg'" mode="aspectFit"></image>
						<image v-else class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'" mode="aspectFit">
						</image>
					</u-grid-item>
					<u-grid-item>
						<image class="taluopaiBG" :src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'"
							mode="aspectFit"></image>
					</u-grid-item>
					<u-grid-item>
						<!-- 4 -->
						<image v-if="index >= 4" class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_bg.jpg'" mode="aspectFit"></image>
						<image v-else class="taluopaiBG"
							:src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'" mode="aspectFit">
						</image>
					</u-grid-item>
					<u-grid-item>
						<image class="taluopaiBG" :src="api_url + '/uploads/xcx/taluopai/taluopai_none_bg.jpg'"
							mode="aspectFit"></image>
					</u-grid-item>
				</u-grid>
			</view>
			<view class="taluopaiBtn">
				<u-button type="success" :disabled="index == 5 ? false : true" @click="getList">翻牌解读</u-button>
			</view>
		</block>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				html:'',
				api_name: '塔罗牌',
				api_url: app.globalData.api_url,
				pai: 22,
				index: 0,
				zhen: 'da_shi_zi',
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
				//开始查询
				that.post('sm_cx/ta_luo_pai', {
					zhen: that.zhen
				}, res => {
					that.list = res.result;
				});
			},
			changeXiPai() {
				this.pai = 22;
				this.index = 0;
				this.list = [];
			},
			delPai() {
				let that = this;
				if (that.index != 5) {
					that.index = that.index + 1;
					that.pai = that.pai - 1;
				} else {
					that.showMsg('只需选5张牌');
					return false;
				}
			}
		}
	}
</script>

<style>
	.taluopaiBox {
		display: flex;
		flex-direction: row;
		width: 100%;
		height: 100rpx;
		box-sizing: border-box;
	}

	.taluopaoList {
		box-sizing: border-box;
		padding: 5rpx;
	}

	.taluopaoList image {
		width: 60rpx;
		height: 100rpx;
	}

	.taluopaiShiZi {
		width: 90%;
		margin: 0 auto;
	}

	.taluopaiBtn {
		width: 90%;
		margin: 30rpx auto;
	}

	.taluopaiBG {
		width: 60rpx;
		height: 60rpx;
	}

	.fanzhuan {
		transform: rotate(-180deg);
	}

	.soList image {
		width: 200rpx;
		height: 300rpx;
	}
</style>
