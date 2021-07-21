<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">汉字：</view>
						<view class="soInputValue">
							<u-input :border="true" maxlength="1" v-model="zi" placeholder="请输入您指定汉字 (限输入1个)"></u-input>
						</view>
					</view>					
					<view class="soInput">
						<view class="soInputLabel">字数：</view>
						<view class="soInputValue">
							<u-number-box v-model="zishu" :min="2" :max="4"></u-number-box>
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
					<u-col span="6">
						<view class="soListTitle">
							<text class="c-primary">{{v.ci}}</text>
							<text v-if="v.detail">
								<u-tag :text="v.detail" size="mini" mode="light" type="error" />
							</text>
						</view>
					</u-col>
					<u-col span="6">
						<view class="soListTitle u-text-right">
							<text>是否注册过：</text>
							<text class="c-info" v-if="v.zhu_ce_guo == 1">是</text>
							<text class="c-success" v-else>否</text>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="0">				
					<u-col span="6">
						<u-line-progress :striped-active="true" :striped="true" active-color="#fa3534" :percent="v.re_du"></u-line-progress>
					</u-col>
				</u-row>
				<u-row gutter="0" class="u-m-b-10 u-m-t-10" v-if="Object.keys(v.zhu_ce_lei_bie).length > 0">
					<u-col span="12">
						<view class="soListTitle">
							<text>已注册分类：</text>
							<block v-for="(vv,ii) in v.zhu_ce_lei_bie" :key="ii" v-if="vv">
								<u-tag :text="vv" size="mini" mode="light" type="info" class="u-m-r-10" />
							</block>
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
				html:'',
				api_name: '商标起名',
				api_url: app.globalData.api_url,
				zi: '',				
				zishu: 2,				
				top: 0,
				winH: app.globalData.winH - 67,
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
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/shang_biao/qi_ming', {
					zi: that.zi,				
					zi_shu: that.zishu
				}, res => {
					that.list = res.data;
				});
			}
		}
	}
</script>

<style>

</style>
