<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel" style="flex:1.4">名称：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="name" placeholder="代理机构名称/申请人名称"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.4;">名称类型：</view>
						<view class="soInputValue">
							<u-input v-model="ctypeLabel" placeholder="请选择名称类型" type="select" :border="true"
								@click="ctypeShow = true" />
							<u-select v-model="ctypeShow" :list="ctypeList" @confirm="changeCType"></u-select>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.4;">查询类型：</view>
						<view class="soInputValue">
							<u-input v-model="stypeLabel" placeholder="请选择查询类型" type="select" :border="true"
								@click="stypeShow = true" />
							<u-select v-model="stypeShow" :list="stypeList" @confirm="changeSType"></u-select>
						</view>
					</view>
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>
				</u-col>
			</u-row>
		</view>
		<block v-if="list.length > 0">
			<u-table>
				<u-tr>
					<u-th>月份</u-th>
					<u-th>{{stypeLabel}}</u-th>
				</u-tr>
				<scroll-view scroll-y :style="'height:'+ winH +'px;'" :scroll-top="top">
					<u-tr v-for="(v,i) in list" :key="i">
						<u-td>{{v.ym}}</u-td>
						<u-td>{{v.tj}}</u-td>
					</u-tr>
				</scroll-view>
			</u-table>
		</block>
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
				api_name: '商标统计',
				api_url: app.globalData.api_url,
				name: '',
				ctype: 0,
				ctypeLabel: '代理机构名称',
				ctypeShow: false,
				ctypeList: [{
						label: '代理机构名称',
						value: 0
					},
					{
						label: '申请人名称',
						value: 1
					}
				],
				stype: 0,
				stypeLabel: '注册量',
				stypeShow: false,
				stypeList: [{
						label: '注册量',
						value: 0
					},
					{
						label: '下证量',
						value: 1
					},
					{
						label: '类别(限用于申请人)',
						value: 2
					}
				],
				top: 0,
				winH: app.globalData.winH - 163,
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
					that.showMsg('请输入代理机构名称/申请人名称');
					return false;
				}
				if (that.ctype == 0 && that.stype == 2) {
					that.showMsg('查询类别(限用于申请人)');
					return false;
				}
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/shang_biao/tong_ji', {
					name: that.name,
					c_type: that.ctype,
					s_type: that.stype
				}, res => {
					if (res.data.length > 0) {
						that.list = res.data;
					} else {
						that.showMsg('无相关数据')
					}
				});
			},
			changeCType(e) {
				this.list = [];
				this.ctype = e[0].value;
				this.ctypeLabel = e[0].label;
			},
			changeSType(e) {
				this.list = [];
				this.stype = e[0].value;
				this.stypeLabel = e[0].label;
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
