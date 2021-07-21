<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">号码：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="haoma" placeholder="请输入测试号码"></u-input>
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
				<view class="soListTitle l-b-t-primary">数理</view>
				<view class="soListContent c-info" v-for="(v,i) in list.shu_li" :key="i">{{v}}</view>
				<view class="soListTitle l-b-t-primary">性格</view>
				<view class="soListContent c-info" v-for="(v,i) in list.xing_ge" :key="i">{{v}}</view>
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
				api_name: '号码吉凶',
				api_url: app.globalData.api_url,
				haoma: '',
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
				if (!that.haoma) {
					that.showMsg('请选输入测试号码');
					return false;
				}
				//开始查询
				that.post('sm_cx/hao_ma', {
					value: that.haoma,
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
