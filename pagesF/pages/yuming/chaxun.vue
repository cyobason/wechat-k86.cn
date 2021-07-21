<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">域名：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="yuming" placeholder="请输入查询域名"></u-input>
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
			<view class="soList" v-for="(v,i) in list" :key="i">
				<view class="soListTitle l-b-t-primary">{{yuming}}</view>
				<view class="soListContent c-error" v-if="v.detail">不可注册【{{v.detail}}】</view>
				<view class="soListContent c-success" v-else>可注册</view>		
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
				api_name: '域名查询',
				api_url: app.globalData.api_url,
				yuming: '',			
				list: [],
				showWin:false
			}
		},
		onLoad() {
			//获取当前功能信息和配置分享信息
			this.getApi(this.api_name);
		},
		methods: {
			getList() {
				let that = this;				
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/yu_ming/cha_xun', {
					yu_ming: that.yuming
				}, res => {
					that.showWin = true;
					that.list = res.data;
				});
			}
		}
	}
</script>

<style>

</style>