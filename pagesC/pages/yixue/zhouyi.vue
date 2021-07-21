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
				<view class="soListTitle l-b-t-primary">卦名</view>
				<view class="soListContent c-info">
					{{list.gua_ming}}
				</view>
				<view class="soListTitle l-b-t-primary">卦符</view>
				<view class="soListContent c-info">
					{{list.fu_hao}}
				</view>
				<view class="soListTitle l-b-t-primary">吉凶</view>
				<view class="soListContent c-info">
					{{list.ji_xiong}}
				</view>
				<view class="soListTitle l-b-t-primary">命格</view>
				<view class="soListContent c-info">					
					<u-parse :html="nl2br(list.ming_ge)"></u-parse>
				</view>
				<view class="soListTitle l-b-t-primary">卦象</view>
				<view class="soListContent c-info" v-for="(v,i) in list.gua_xiang" :key="i">
					{{i}}：<view class="soListContent c-info" v-for="(vv,ii) in v" :key="ii">{{vv}}</view>
				</view>
				<view class="soListTitle l-b-t-primary">详解</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.xiang_jie)"></u-parse>
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
				api_name: '周易算命',
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
				that.post('sm_cx/zhou_yi', {
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
