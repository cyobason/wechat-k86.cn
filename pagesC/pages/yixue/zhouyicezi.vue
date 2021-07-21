<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">测字：</view>
						<view class="soInputValue">
							<u-input maxlength="2" :border="true" v-model="zi" placeholder="请输入要测的字"></u-input>
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
				<view class="soListTitle l-b-t-primary">测字</view>
				<view class="soListContent c-info">{{list.zi.join(' ')}}</view>
				<view class="soListTitle l-b-t-primary">拼音</view>
				<view class="soListContent c-info">{{list.pin_yin.join(' ')}}</view>
				<view class="soListTitle l-b-t-primary">签语</view>
				<view class="soListContent c-info">{{list.result}}</view>
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
				api_name: '周易测字',
				api_url: app.globalData.api_url,
				zi: '',
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
				if (!that.zi) {
					that.showMsg('请输入要测的字');
					return false;
				}
				//开始查询
				that.post('sm_cx/zhou_yi_ce_zi', {
					zi: that.zi,
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			}
		}
	}
</script>

<style>

</style>
