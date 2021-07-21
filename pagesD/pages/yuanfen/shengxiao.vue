<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">您的生日：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen1" @click="showShiChen1 = true" type="select"
								placeholder="请选择您的农历年"></u-input>
							<u-picker v-model="showShiChen1" mode="time" :params="shichenList"
								@confirm="changeShiChen1">
							</u-picker>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">对方生日：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen2" @click="showShiChen2 = true" type="select"
								placeholder="请选择对方农历年"></u-input>
							<u-picker v-model="showShiChen2" mode="time" :params="shichenList"
								@confirm="changeShiChen2">
							</u-picker>
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
				<view class="soListTitle l-b-t-primary">生肖配对</view>
				<view class="soListContent c-info" v-for="(v,i) in list.sheng_xiao_pei_dui" :key="i" v-if="v"> {{v}}
				</view>
				<view class="soListTitle l-b-t-primary">传统婚配</view>
				<view class="soListContent c-info" v-for="(v,i) in list.chuan_tong_hun_pei" :key="i" v-if="v"> {{v}}
				</view>
				<view class="soListTitle l-b-t-primary">盲派婚配</view>
				<view class="soListContent c-info" v-for="(v,i) in list.mang_pai_hun_pei" :key="i" v-if="v"> {{v}}
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
				api_name: '生肖配对',
				api_url: app.globalData.api_url,
				shichen1: '',
				showShiChen1: false,
				shichenList: {
					year: true,
					month: false,
					day: false,
					hour: false,
					minute: false,
					second: false,
					timestamp: true
				},
				shichen2: '',
				showShiChen2: false,
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
				if (!that.shichen1) {
					that.showMsg('请选择您的农历年');
					return false;
				}
				if (!that.shichen2) {
					that.showMsg('请选择对方农历年');
					return false;
				}
				//开始查询
				that.post('yuan_fen/sheng_xiao', {
					nian1: that.shichen1,
					nian2: that.shichen2
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen1(e) {
				this.shichen1 = e.year;
			},
			changeShiChen2(e) {
				this.shichen2 = e.year;
			},
		}
	}
</script>

<style>
	/* 重写样式 */
	.soBtn {		
		height: 164rpx;
	}
</style>
