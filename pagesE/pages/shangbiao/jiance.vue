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
		<scroll-view scroll-y :style="'height:'+ winH +'px;'" :scroll-top="top" v-if="Object.keys(list).length > 0">
			<view class="tips-primary">
				<view class="soListTitle">
					<text>商标局最新公告期数：</text>
					<text class="c-error">{{list.last.ann_num}}期</text>
				</view>
				<view class="soListTitle">
					<text>商标局最新公告日期：</text>
					<text class="c-info">{{list.last.ann_date}}</text>
				</view>
			</view>
			<view class="soList" v-for="(v,i) in list.result" :key="i" v-if="list.result">				
				<u-row gutter="0">
					<u-col span="12">
						<view class="soListTitle">
							<text class="c-primary">{{v.tm_name}}</text>						
						</view>
					</u-col>
				</u-row>
				<u-row gutter="0">
					<u-col span="12">
						<view class="soListTitle">
							<text>申请号：</text>
							<text class="c-info">{{v.reg_no}}</text>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="0">
					<u-col span="12">
						<view class="soListTitle">
							<text>公告类型：</text>
							<text class="c-info">{{v.ann_type_code}}</text>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="0">
					<u-col span="6">
						<view class="soListTitle">
							<text>公告期数：</text>
							<text class="c-info">{{v.ann_num}}期</text>
						</view>
					</u-col>
					<u-col span="6">
						<view class="soListTitle u-text-right">
							<text>公告时间：</text>
							<text class="c-info">{{v.ann_date}}</text>
						</view>
					</u-col>
				</u-row>				
			</view>
			<view style="height: 30%;">
				<u-empty text="没有相关公告" mode="search" v-if="!list.result"></u-empty>
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
				api_name: '商标监测',
				api_url: app.globalData.api_url,
				name: '',
				page: 1,
				top: 0,
				winH: app.globalData.winH - 66,
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
				if (!that.name) {
					that.showMsg('请输入申请人名称');
					return false;
				}
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/shang_biao/jian_ce', {
					name: that.name,
					page: that.page
				}, res => {
					that.list = res.data;
				});
			}
		}
	}
</script>

<style>

</style>
