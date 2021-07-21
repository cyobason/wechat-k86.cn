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
				<view class="soListTitle u-text-center">{{list.du + '：' + list.result.ming}}</view>
				<view class="soListTitle l-b-t-primary">阳历</view>
				<view class="soListContent c-info">
					{{list.info.yang_li}}
				</view>
				<view class="soListTitle l-b-t-primary">农历</view>
				<view class="soListContent c-info">
					{{list.info.nong_li}}
				</view>
				<view class="soListTitle l-b-t-primary">生肖</view>
				<view class="soListContent c-info">
					{{list.info.sheng_xiao}}
				</view>				
				<view class="soListTitle l-b-t-primary">财运</view>
				<view class="soListContent c-info">
					{{list.result.cai_yun}}
				</view>
				<view class="soListTitle l-b-t-primary">婚姻</view>
				<view class="soListContent c-info">
					{{list.result.hui_yin}}
				</view>
				<view class="soListTitle l-b-t-primary">禄位</view>
				<view class="soListContent c-info">
					{{list.result.lu_wei}}
				</view>
				<view class="soListTitle l-b-t-primary">诗曰</view>
				<view class="soListContent c-info">
					{{list.result.shi_yue}}
				</view>
				<view class="soListTitle l-b-t-primary">子息</view>
				<view class="soListContent c-info">
					{{list.result.zi_xi}}
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
				api_name: '宫度论命',
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
				if (!that.shichen) {
					that.showMsg('请选择您的阳历生日');
					return false;
				}
				//开始查询
				that.post('sm_cx/gong_du', {					
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
