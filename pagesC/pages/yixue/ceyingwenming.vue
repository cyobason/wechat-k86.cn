<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.2;">英文名：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="ming" placeholder="请输入要测的英文名"></u-input>
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
				<view class="soListTitle l-b-t-primary">名字独白</view>
				<view class="soListContent c-info" v-for="(v,i) in list.du_bai" :key="i">
					<view>{{list.letter[i].toUpperCase()}} —— {{v[0]}} {{v[1]}}</view>
					<view>在英语中字母 <text class="c-error u-m-l-10 u-m-r-10 bold">{{list.letter[i]}}</text> 的使用频率为：<text
							class="c-error">{{v[2]}}%</text></view>
				</view>
				<view class="soListTitle l-b-t-primary">名字性格</view>
				<view class="soListContent c-info" v-for="(v,i) in list.xing_ge" :key="i">
					{{v}}
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
				api_name: '测英文名',
				api_url: app.globalData.api_url,
				ming: '',
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
				if (!that.ming) {
					that.showMsg('请输入要测的英文名');
					return false;
				}
				//开始查询
				that.post('sm_cx/ce_ying_wen_ming', {
					ming: that.ming,
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
