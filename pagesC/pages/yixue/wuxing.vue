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
					<view class="soInput">
						<view class="soInputLabel">性别：</view>
						<view class="soInputValue">
							<view class="u-m-r-10 c-info">女</view>
							<u-switch v-model="sex" active-value="1" inactive-value="0" @change="changeSex">
							</u-switch>
							<view class="u-m-l-10 c-info">男</view>
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
				<view class="soListTitle l-b-t-primary">农历</view>
				<view class="soListContent c-info">{{list.nong_li.nian}}年{{list.nong_li.yue}}月{{list.nong_li.ri}}
					{{list.nong_li.shi}}时
				</view>
				<view class="soListTitle l-b-t-primary">生肖</view>
				<view class="soListContent c-info">{{list.sheng_xiao}}</view>
				<view class="soListTitle l-b-t-primary">八字</view>
				<view class="soListContent c-info">{{list.ba_zi.nian}}年{{list.ba_zi.yue}}月{{list.ba_zi.ri}}日
					{{list.ba_zi.shi}}时
				</view>
				<view class="soListTitle l-b-t-primary">五行个数</view>
				<view class="soListContent c-info u-flex u-flex-wrap">
					<view v-for="(v,i) in list.wu_xing_ge_shu" :key="i" class="u-row-left u-m-r-20 u-m-b-10">
						<u-tag :text="v + '个' + i" />
					</view>
				</view>
				<view class="soListTitle l-b-t-primary">五行缺</view>
				<view class="soListContent c-info">{{list.wu_xing_qie}}</view>
				<view class="soListTitle l-b-t-primary">五行旺</view>
				<view class="soListContent c-info">{{list.wu_xing_wang}}</view>
				<view class="soListTitle l-b-t-primary">帮扶</view>
				<view class="soListContent c-info">{{list.bang_fu}}</view>
				<view class="soListTitle l-b-t-primary">克泄耗</view>
				<view class="soListContent c-info">{{list.ke_xie_hao}}</view>
				<view class="soListTitle l-b-t-primary">穷通宝鉴论命</view>
				<view class="soListContent c-info">{{list.qiong_tong_bao_jian}}</view>
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
				api_name: '五行查询',
				api_url: app.globalData.api_url,
				sex: 0,
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
				that.post('sm_cx/wu_xing', {
					date: that.shichen,
					sex: that.sex
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
