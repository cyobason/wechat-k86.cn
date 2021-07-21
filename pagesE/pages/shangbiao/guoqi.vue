<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">名称：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="name" placeholder="请输入申请人名称"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">时间：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="time" @click="showTime = true" type="select"
								placeholder="请选择日期范围"></u-input>
							<u-calendar v-model="showTime" mode="range" @change="changeTime" max-date="2050-12-31">
							</u-calendar>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">页码：</view>
						<view class="soInputValue">
							<u-number-box v-model="page" :min="1" :max="1"></u-number-box>
						</view>
					</view>
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>
				</u-col>
			</u-row>
		</view>
		<scroll-view scroll-y :style="'height:'+ winH +'px;'" :scroll-top="top" v-if="list.length > 0">
			<view class="soList" v-for="(v,i) in list" :key="i">
				<u-row gutter="0">
					<u-col span="12">
						<view class="soListTitle">
							<text class="c-primary">{{v.name}}</text>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="0">
					<u-col span="6">
						<view class="soListTitle">
							<text>申请号：</text>
							<text class="c-info">{{v.reg_no}}</text>
						</view>
					</u-col>
					<u-col span="6">
						<view class="soListTitle u-text-right">
							<text>商标分类：</text>
							<text class="c-info">{{v.cls}}类</text>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="0">
					<u-col span="12">
						<view class="soListTitle">
							<text>申请人：</text>
							<text class="c-info">{{v.contact}}</text>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="0">
					<u-col span="6">
						<view class="soListTitle">
							<text>注册时间：</text>
							<text class="c-info">{{v.reg}}</text>
						</view>
					</u-col>
					<u-col span="6">
						<view class="soListTitle u-text-right">
							<text>过期时间：</text>
							<text class="c-info">{{v.expire}}</text>
						</view>
					</u-col>
				</u-row>
			</view>
		</scroll-view>
		<view class="tips-warning" v-else>
			<u-parse :html="html"></u-parse>
		</view>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				html: '',
				api_name: '过期查询',
				api_url: app.globalData.api_url,
				time: '',
				showTime: false,
				start: '',
				end: '',
				name: '',
				page: 1,
				top: 0,
				winH: app.globalData.winH - 117,
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
				if (!that.time || !that.start || !that.end) {
					that.showMsg('请选择日期范围');
					return false;
				}
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/shang_biao/guo_qi', {
					name: that.name,
					start: that.start,
					end: that.end,
					page: that.page
				}, res => {
					if (Object.keys(res.data).length > 0) {
						that.list = res.data;
					}else{
						that.showMsg('没有相关数据');
					}
				});
			},
			changeTime(e) {
				this.time = e.startDate + ' ~ ' + e.endDate;
				this.start = e.startDate;
				this.end = e.endDate;
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
