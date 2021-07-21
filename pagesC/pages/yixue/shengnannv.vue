<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">月份：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen" @click="showShiChen = true" type="select"
								placeholder="请选择农历月份"></u-input>
							<u-picker v-model="showShiChen" mode="time" :params="shichenList" @confirm="changeShiChen">
							</u-picker>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.2;">虚岁：</view>
						<view class="soInputValue">
							<u-number-box v-model="sui" :min="18" :max="44"></u-number-box>
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
					<view class="c-info">注意：参数条件 月份, 男/女 二选一。</view>
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>					
				</u-col>
			</u-row>
		</view>
		<u-popup v-model="showWin" v-if="Object.keys(list).length > 0" border-radius="20" mode="center" width="95%"
			:closeable="true">
			<view class="soList">
				<block v-if="shichen && list.result">
					<view class="soListTitle">
						{{list.result}}
					</view>
				</block>
				<block v-if="shichen == '' && list.result.jin_nian.length > 0">
					<view class="soListTitle u-text-center">如果想生{{sex == 0 ? '女' : '男'}}宝宝，农历几月受孕几率会大一点？</view>					
					<view class="soListTitle l-b-t-primary">今年</view>
					<view class="soListTitle u-flex u-flex-wrap">
						<view v-for="(v,i) in list.result.jin_nian" :key="i" class="u-row-left u-m-r-20 u-m-t-20">
							<u-tag :text="v + '月'"/>
						</view>
					</view>
					<view class="soListTitle l-b-t-primary">明年</view>
					<view class="soListTitle u-flex u-flex-wrap">
						<view v-for="(v,i) in list.result.ming_nian" :key="i" class="u-row-left u-m-r-20 u-m-t-20">
							<u-tag :text="v + '月'" type="success" />
						</view>
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
				api_name: '生男生女',
				api_url: app.globalData.api_url,
				sui: 18,
				sex: 0,
				shichen: '',
				showShiChen: false,
				shichenList: {
					year: false,
					month: true,
					day: false,
					hour: false,
					minute: false,
					second: false,
					timestamp: true
				},
				showWin:false,
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
				//开始查询
				that.post('sm_cx/sheng_nan_nv', {
					sui: that.sui,
					want: that.sex,
					month: that.shichen == '' ? 0 : that.shichen,
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen(e) {
				this.list = [];
				this.shichen = e.month;
				this.sex = 0;
			},
			changeSex(e) {
				this.list = [];
				this.sex = e;
				this.shichen = '';
			}
		}
	}
</script>

<style>
	
</style>
