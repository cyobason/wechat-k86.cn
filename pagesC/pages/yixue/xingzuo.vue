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
				<view class="soListTitle l-b-t-primary">星座</view>
				<view class="soListContent c-info">
					{{list.xing_zuo}}
				</view>
				<block v-if="list.shang_sheng_xing_zuo">
					<view class="soListTitle l-b-t-primary">上升星座</view>
					<view class="soListContent c-info">
						{{list.shang_sheng_xing_zuo}}
					</view>
				</block>
				<block v-if="list.shang_sheng_xing_zuo_te_dian">
					<view class="soListTitle l-b-t-primary">上升星座特点</view>
					<view class="soListContent c-info">
						<block v-for="(v,i) in list.shang_sheng_xing_zuo_te_dian" :key="i">
							<u-line class="u-line" border-style="dashed" margin="20rpx 0" v-if="i != 0"
								:hair-line="false">
							</u-line>
							<u-parse :html="nl2br(v)"></u-parse>
						</block>
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
				api_name: '星座查询',
				api_url: app.globalData.api_url,
				month: '',
				day: '',
				shichen: '',
				showShiChen: false,
				shichenList: {
					year: false,
					month: true,
					day: true,
					hour: false,
					minute: false,
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
				that.post('sm_cx/xing_zuo', {
					month: that.month,
					day: that.day
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen(e) {
				this.shichen = e.month + '-' + e.day;
				this.month = e.month;
				this.day = e.day;
			}
		}
	}
</script>

<style>
	
</style>
