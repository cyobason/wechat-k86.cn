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
				<view class="soListTitle l-b-t-primary">星宿</view>
				<view class="soListContent c-info">
					{{list.xing_xiu}}宿（{{list.xing_xiu}}{{list.wu_xing}}{{list.dong_wu}}）
				</view>
				<view class="soListTitle l-b-t-primary">歌谣</view>
				<view class="soListContent c-info">
					{{list.ge_yao}}
				</view>
				<block v-for="(v,i) in list.zi_duan" :key="i">
					<view class="soListTitle l-b-t-primary">{{v}}</view>
					<view class="soListContent c-info" v-for="(vv,ii) in list.xiang_jie[i]" :key="ii">
						{{vv}}
					</view>
				</block>
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
				api_name: '二十八星宿',
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
				that.post('sm_cx/xing_xiu', {
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
