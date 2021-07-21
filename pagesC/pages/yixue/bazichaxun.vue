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
				<view class="soListTitle l-b-t-primary">日柱命理</view>
				<view class="soListContent c-info">
					{{list.ri_zhu_ming_li}}
				</view>
				<view class="soListTitle l-b-t-primary">日元命理</view>
				<view class="soListContent c-info">
					{{list.ri_yuan_ming_li}}
				</view>
				<view class="soListTitle l-b-t-primary">三命通会</view>
				<view class="soListContent c-info" v-for="(v,i) in list.san_ming_tong_hui" :key="i">
					{{v}}
				</view>
				<view class="soListTitle l-b-t-primary">天干命格</view>
				<view class="soListContent c-info" v-for="(v,i) in list.tian_gan_ming_ge" :key="i">
					{{v}}
				</view>
				<view class="soListTitle l-b-t-primary">生肖性格</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.sheng_xiao_xing_ge)"></u-parse>
				</view>
				<view class="soListTitle l-b-t-primary">运程</view>
				<view class="soListContent c-info" v-for="(v,i) in list.yun_cheng" :key="i">
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
				api_name: '八字查询',
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
				that.post('sm_cx/ba_zi', {
					sex: that.sex,
					date: that.shichen,
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen(e) {
				this.shichen = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':' + e.minute;
			},
			changeSex(e) {
				this.sex = e;
			}
		}
	}
</script>

<style>
	
</style>
