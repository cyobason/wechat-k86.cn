<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">指纹：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="wuLabel" @click="wuShow = true" type="select"
								placeholder="请选择五指指纹"></u-input>
							<u-select v-model="wuShow" mode="mutil-column" :list="wuList" @confirm="changeWu">
							</u-select>
						</view>
					</view>
					<view class="c-info">指纹顺序：大拇指,食指,中指,无名指,小指</view>
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>
				</u-col>
			</u-row>
		</view>
		<u-popup v-model="showWin" v-if="Object.keys(list).length > 0" border-radius="20" mode="center" width="95%"
			:closeable="true">
			<view class="soList">
				<view class="soListTitle l-b-t-primary">测试结果</view>
				<view class="soListContent">
					<u-parse :html="nl2br(list.result)"></u-parse>
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
				api_name: '指纹测试',
				api_url: app.globalData.api_url,	
				wuLabel: '',
				wuValue: '',
				wuShow: false,
				wuList: [
					[{
						label: '涡纹',
						value: 'O'
					}, {
						label: '流紋',
						value: 'X'
					}],
					[{
						label: '涡纹',
						value: 'O'
					}, {
						label: '流紋',
						value: 'X'
					}],
					[{
						label: '涡纹',
						value: 'O'
					}, {
						label: '流紋',
						value: 'X'
					}],
					[{
						label: '涡纹',
						value: 'O'
					}, {
						label: '流紋',
						value: 'X'
					}],
					[{
						label: '涡纹',
						value: 'O'
					}, {
						label: '流紋',
						value: 'X'
					}]
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
				if (!that.wuValue) {
					that.showMsg('请选择五指指纹');
					return false;
				}		
				//开始查询
				that.post('sm_cx/zhi_wen', {
					wu: that.wuValue,
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeWu(e) {
				this.wuLabel = e[0].label + ',' + e[1].label + ',' + e[2].label + ',' + e[3].label + ',' + e[4].label;
				this.wuValue = e[0].value + e[1].value + e[2].value + e[3].value + e[4].value;
			}
		}
	}
</script>

<style>
	
</style>
