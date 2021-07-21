<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">生日：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen" @click="showShiChen = true" type="select"
								placeholder="请选择您的阳历生日"></u-input>
							<u-picker v-model="showShiChen" mode="time" :params="shichenList" @confirm="changeShiChen">
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
				<view class="soListTitle l-b-t-primary">前世今生</view>
				<view class="soListContent c-info">
					{{list.qian_shi_jin_sheng}}
				</view>
				<view class="soListTitle l-b-t-primary">您的财禄</view>
				<view class="soListContent c-info">
					{{list.cai_lu.lu}}：{{list.cai_lu.lu_ming}}
				</view>
				<view class="soListTitle l-b-t-primary">财禄诗曰</view>
				<view class="soListContent c-info" v-for="(v,i) in list.cai_lu.cai_lu_shi_yue" :key="i">
					{{v}}
				</view>
				<view class="soListTitle l-b-t-primary">财禄诗义</view>
				<view class="soListContent c-info">
					{{list.cai_lu.cai_lu_shi_yi}}
				</view>
				<view class="soListTitle l-b-t-primary">您的官禄</view>
				<view class="soListContent c-info">
					{{list.guan_lu.lu}}
				</view>
				<view class="soListTitle l-b-t-primary">前世命定</view>
				<view class="soListContent c-info">
					{{list.guan_lu.qian_shi_ming_ding}}
				</view>
				<view class="soListTitle l-b-t-primary">现世托化</view>
				<view class="soListContent c-info">
					{{list.guan_lu.xian_shi_tuo_hua}}
				</view>
				<view class="soListTitle l-b-t-primary">影响今世</view>
				<view class="soListContent c-info">
					{{list.guan_lu.ying_xiang_jin_shi}}
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
				api_name: '三世书测财运',
				api_url: app.globalData.api_url,
				shichen: '',
				showShiChen: false,
				shichenList: {
					year: true,
					month: true,
					day: true,
					hour: true,
					minute: true,
					second: false,
					timestamp: true
				},
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
				if (!that.shichen) {
					that.showMsg('请选择您的阳历生日');
					return false;
				}
				//开始查询
				that.post('sm_cx/san_shi_shu_cai_yun', {
					date: that.shichen,
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen(e) {
				this.shichen = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':' + e.minute;
			}
		}
	}
</script>

<style>
	
</style>
