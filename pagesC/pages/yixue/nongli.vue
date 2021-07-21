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
				<view class="soListTitle l-b-t-primary">月命格</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.yue_ming_ge)"></u-parse>
				</view>
				<view class="soListTitle l-b-t-primary">月生肖</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.yue_sheng_xiao)"></u-parse>
				</view>
				<view class="soListTitle l-b-t-primary">日命格</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.ri_ming_ge)"></u-parse>
				</view>
				<view class="soListTitle l-b-t-primary">日生肖</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.ri_sheng_xiao)"></u-parse>
				</view>
				<view class="soListTitle l-b-t-primary">时命格</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.shi_ming_ge)"></u-parse>
				</view>
				<view class="soListTitle l-b-t-primary">时生肖</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.shi_sheng_xiao)"></u-parse>
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
				api_name: '农历算命',
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
				that.post('sm_cx/nong_li', {
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
