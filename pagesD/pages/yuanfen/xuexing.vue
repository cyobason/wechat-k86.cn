<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">您的血型：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="xuexing1" @click="showXueXing1 = true" type="select"
								placeholder="请选择您的血型"></u-input>
							<u-select v-model="showXueXing1" :list="xuexingList" @confirm="changeXueXing1"></u-select>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">对方血型：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="xuexing2" @click="showXueXing2 = true" type="select"
								placeholder="请选择对方血型"></u-input>
							<u-select v-model="showXueXing2" :list="xuexingList" @confirm="changeXueXing2"></u-select>
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
			<view class="soList">
				<view class="soListTitle l-b-t-primary">缘分</view>
				<view class="soListContent c-info">
					{{list.yuan_fen}}
				</view>
				<view class="soListTitle l-b-t-primary">详解</view>
				<view class="soListContent c-info">
					{{list.xiang_jie}}
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
				api_name: '血型配对',
				api_url: app.globalData.api_url,
				xuexing1: '',
				xuexing2: '',
				xuexingList: [{
						value: 'A',
						label: 'A'
					},
					{
						value: 'AB',
						label: 'AB'
					},
					{
						value: 'B',
						label: 'B'
					},
					{
						value: 'O',
						label: 'O'
					}
				],
				showWin: false,
				showXueXing1: false,
				showXueXing2: false,
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
				if (!that.xuexing1) {
					that.showMsg('请选择您的血型');
					return false;
				}
				if (!that.xuexing2) {
					that.showMsg('请选择对方血型');
					return false;
				}
				//开始查询
				that.post('yuan_fen/xue_xing', {
					xx1: that.xuexing1,
					xx2: that.xuexing2,
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeXueXing1(e) {
				this.xuexing1 = e[0].value;
			},
			changeXueXing2(e) {
				this.xuexing2 = e[0].value;
			}
		}
	}
</script>

<style>
	/* 重写样式 */
	.soBtn{
		height: 164rpx;
	}
</style>
