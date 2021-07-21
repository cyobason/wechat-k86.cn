<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">占卜：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="zhanbuLabel" @click="zhanbuShow = true" type="select"
								placeholder="请选择占卜类型"></u-input>
							<u-select v-model="zhanbuShow" :list="zhanbuList" @confirm="changeZhanBu"></u-select>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">时辰：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichenLabel" @click="shichenShow = true" type="select"
								placeholder="请选择时辰"></u-input>
							<u-select v-model="shichenShow" :list="shichenList" @confirm="changeShiChen"></u-select>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">位置：</view>
						<view class="soInputValue">
							<view class="u-m-r-10 c-info">左眼或左耳</view>
							<u-switch v-model="place" active-value="1" inactive-value="0" @change="changePlace">
							</u-switch>
							<view class="u-m-l-10 c-info">右眼或右耳</view>
						</view>
					</view>
					<view class="c-info">注意：仅用于眼跳吉凶和耳鸣占卜</view>
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>
				</u-col>
			</u-row>
		</view>
		<u-popup v-model="showWin" v-if="Object.keys(list).length > 0" border-radius="20" mode="center" width="95%"
			:closeable="true">
			<view class="soList">
				<view class="soListTitle">
					{{list.result}}
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
				api_name: '占卜吉凶',
				api_url: app.globalData.api_url,
				place: 0,
				zhanbuLabel: '',
				zhanbuValue: '',
				zhanbuShow: false,
				zhanbuList: [{
						label: '眼跳吉凶',
						value: '眼跳吉凶'
					},
					{
						label: '喷嚏吉凶',
						value: '喷嚏吉凶'
					},
					{
						label: '心惊吉凶',
						value: '心惊吉凶'
					},
					{
						label: '肉跳吉凶',
						value: '肉跳吉凶'
					},
					{
						label: '面热占卜',
						value: '面热占卜'
					},
					{
						label: '耳热占卜',
						value: '耳热占卜'
					},
					{
						label: '衣留占卜',
						value: '衣留占卜'
					},
					{
						label: '鹊噪占卜',
						value: '鹊噪占卜'
					},
					{
						label: '耳鸣占卜',
						value: '耳鸣占卜'
					},
					{
						label: '釜鸣占卜',
						value: '釜鸣占卜'
					},
					{
						label: '火逸占卜',
						value: '火逸占卜'
					},
					{
						label: '犬嚎占卜',
						value: '犬嚎占卜'
					}
				],
				shichenLabel: '',
				shichenValue: '',
				shichenShow: false,
				shichenList: [{
						label: '子时（23点－凌晨1点前）',
						value: '子'
					},
					{
						label: '丑时（1点－凌晨3点前）',
						value: '丑'
					},
					{
						label: '寅时（3点－凌晨5点前）',
						value: '寅'
					},
					{
						label: '卯时（5点－上午7点前）',
						value: '卯'
					},
					{
						label: '辰时（7点－上午9点前）',
						value: '辰'
					},
					{
						label: '巳时（9点－上午11点前）',
						value: '巳'
					},
					{
						label: '午时（11点－中午13点前）',
						value: '午'
					},
					{
						label: '未时（13点－下午15点前）',
						value: '未'
					},
					{
						label: '申时（15点－下午17点前）',
						value: '申'
					},
					{
						label: '酉时（17点－下午19点前）',
						value: '酉'
					},
					{
						label: '戌时（19点－晚上21点前）',
						value: '戌'
					},
					{
						label: '亥时（21点－晚上23点前）',
						value: '亥'
					}
				],
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
				if (!that.zhanbuValue) {
					that.showMsg('请选择占卜类型');
					return false;
				}
				if (!that.shichenValue) {
					that.showMsg('请选择时辰');
					return false;
				}
				//开始查询
				that.post('sm_cx/zhan_bu', {
					place: that.place,
					ce: that.zhanbuValue,
					shi_cheng: that.shichenValue,
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen(e) {
				this.shichenLabel = e[0].label;
				this.shichenValue = e[0].value;
			},
			changeZhanBu(e) {
				if (e[0].label != '眼跳吉凶' || e[0].label != '耳鸣占卜') {
					this.place = 0;
				}
				this.zhanbuLabel = e[0].label;
				this.zhanbuValue = e[0].value;
			},
			changePlace(e) {
				this.place = e;
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
