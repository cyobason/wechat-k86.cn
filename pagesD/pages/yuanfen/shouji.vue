<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">您的手机：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shouji1" placeholder="请输入您的手机号"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">对方手机：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shouji2" placeholder="请输入对方手机号"></u-input>
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
				<view class="soListTitle l-b-t-primary">缘分</view>
				<view class="soListContent c-info">
					{{list.yuan_fen}}
				</view>
				<view class="soListTitle l-b-t-primary">婚姻</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.hun_yin}}
				</view>
				<view class="soListTitle l-b-t-primary">健康</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.jian_kang}}
				</view>
				<view class="soListTitle l-b-t-primary">性格</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.xing_ge}}
				</view>
				<view class="soListTitle l-b-t-primary">运势</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.yun_shi}}
				</view>
				<view class="soListTitle l-b-t-primary">职业</view>
				<view class="soListContent c-info">
					{{list.xiang_jie.zhi_ye}}
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
				api_name: '手机配对',
				api_url: app.globalData.api_url,
				shouji1:'',
				shouji2:'',
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
				if (!that.shouji1) {
					that.showMsg('请选输入您的手机号');
					return false;
				}
				if (!that.shouji2) {
					that.showMsg('请选输入对方手机号');
					return false;
				}
				//开始查询
				that.post('yuan_fen/shou_ji', {
					num1: that.shouji1,
					num2: that.shouji2
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			}
		}
	}
</script>

<style>
	/* 重写样式 */
	.soBtn {
		height: 164rpx;
	}
</style>
