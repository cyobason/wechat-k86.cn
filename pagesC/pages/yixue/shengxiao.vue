<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">年份：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen" @click="showShiChen = true" type="select"
								placeholder="请选择出生年份"></u-input>
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
				<view class="soListTitle l-b-t-primary">生肖</view>
				<view class="soListContent c-info">
					{{list.sheng_xiao}}
				</view>
				<view class="soListTitle l-b-t-primary">排行</view>
				<view class="soListContent c-info">
					十二生肖排第{{list.pai_wei}}位
				</view>
				<view class="soListTitle l-b-t-primary">农历</view>
				<view class="soListContent c-info">
					{{list.nong_li}}年
				</view>
				<view class="soListTitle l-b-t-primary">五行</view>
				<view class="soListContent c-info">
					{{list.wu_xing}}
				</view>
				<view class="soListTitle l-b-t-primary">财运</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.cai_yun}}
				</view>
				<view class="soListTitle l-b-t-primary">感情</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.gan_qing}}
				</view>
				<view class="soListTitle l-b-t-primary">事业</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.shi_ye}}
				</view>
				<view class="soListTitle l-b-t-primary">性格</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.xing_ge}}
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
				api_name: '生肖查询',
				api_url: app.globalData.api_url,
				shichen: '',
				showShiChen: false,
				shichenList: {
					year: true,
					month: false,
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
				if (!that.shichen) {
					that.showMsg('请选择出生年份');
					return false;
				}
				//开始查询
				that.post('sm_cx/sheng_xiao', {					
					year: that.shichen
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen(e) {				
				this.shichen = e.year;
			}
		}
	}
</script>

<style>
	
</style>
