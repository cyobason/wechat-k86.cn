<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">姓氏：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="xing" placeholder="请输入您的姓氏"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">名字：</view>
						<view class="soInputValue">
							<u-input maxlength="1" :border="true" v-model="ming" placeholder="请输入名字里面包含的字"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">朝代：</view>
						<view class="soInputValue">
							<u-input v-model="chaodai" placeholder="请选择古诗的朝代" type="select" :border="true"
								@click="showChaoDai = true" />
							<u-select v-model="showChaoDai" :list="chaodaiList" @confirm="changeChaoDai"></u-select>
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
				<text class="soListTitle" :selectable="true" :user-select="true">{{xing + v.word.join("")}}</text>
				<view class="soListContent">
					<view class="u-m-t-10 u-m-b-20 subtitle2">作者：{{v.author}}〔{{v.dynasty}}〕</view>
					<view class="u-m-b-20">出处：{{v.title}}</view>
					<u-parse :html="v.content"></u-parse>
				</view>
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
				html:'',
				api_name: '古诗起名',
				api_url: app.globalData.api_url,
				xing: '',
				ming: '',
				chaodai: '',
				chaodaiList: [{
						label: '先秦',
						value: '先秦'
					},
					{
						label: '秦',
						value: '秦'
					},
					{
						label: '汉',
						value: '汉'
					},
					{
						label: '魏晋',
						value: '魏晋'
					},
					{
						label: '魏晋末南北朝初',
						value: '魏晋末南北朝初'
					},
					{
						label: '南北朝',
						value: '南北朝'
					},
					{
						label: '隋',
						value: '隋'
					},
					{
						label: '隋末唐初',
						value: '隋末唐初'
					},
					{
						label: '唐',
						value: '唐'
					},
					{
						label: '唐末宋初',
						value: '唐末宋初'
					},
					{
						label: '宋',
						value: '宋'
					},
					{
						label: '宋末元初',
						value: '宋末元初'
					},
					{
						label: '宋末金初',
						value: '宋末金初'
					},
					{
						label: '辽',
						value: '辽'
					},
					{
						label: '金',
						value: '金'
					},
					{
						label: '金末元初',
						value: '金末元初'
					},
					{
						label: '元',
						value: '元'
					},
					{
						label: '元末明初',
						value: '元末明初'
					},
					{
						label: '明',
						value: '明'
					},
					{
						label: '明末清初',
						value: '明末清初'
					},
					{
						label: '清',
						value: '清'
					},
					{
						label: '清末民国初',
						value: '清末民国初'
					},
					{
						label: '清末近现代初',
						value: '清末近现代初'
					},
					{
						label: '民国末当代初',
						value: '民国末当代初'
					},
					{
						label: '近现代',
						value: '近现代'
					},
					{
						label: '近现代末当代初',
						value: '近现代末当代初'
					},
					{
						label: '当代',
						value: '当代'
					}
				],
				showChaoDai: false,
				top: 0,
				winH: app.globalData.winH - 134,
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
				if (!that.xing) {
					that.showMsg('请输入您的姓氏');
					return false;
				}
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/qi_ming/gu_shi', {
					word: that.ming,
					zLen: 2,
					dynasty: that.chaodai
				}, res => {
					that.list = res;
					that.list.forEach((v, i) => {
						v.word.forEach((vv, ii) => {
							that.list[i].content = v.content.replace(vv,
								'<view class="c-error">' + vv + '</view>');
						});
					});
				});
			},
			changeChaoDai(e) {
				this.chaodai = e[0].value;
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
