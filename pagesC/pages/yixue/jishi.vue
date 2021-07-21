<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">日期：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen" @click="showShiChen = true" type="select"
								placeholder="请选择日期范围"></u-input>
							<u-calendar v-model="showShiChen" mode="range" @change="changeShiChen"
								max-date="2050-12-31"></u-calendar>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">术语：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="labelText" @click="labelShow = true" type="select"
								placeholder="请选择相关术语"></u-input>
							<u-select v-model="labelShow" mode="single-column" :list="labelList" @confirm="changelabel">
							</u-select>
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
			<view class="soList" v-for="(v,i) in list.result" :key="i">
				<view class="soListTitle l-b-t-primary">{{v.date}} {{v.nong_li}} {{v.xing_qi}}</view>
				<view class="soListContent">
					<block v-if="v.ji_shi[0] == '全天'">
						<view class="c-error bold">百无禁忌</view>
					</block>
					<block v-else>
						<u-table>
							<u-tr>
								<u-th>时刻</u-th>
								<u-th>时辰</u-th>
								<u-th>冲煞</u-th>
								<u-th>煞方</u-th>
							</u-tr>
							<u-tr v-for="(vv,ii) in v.ji_shi" :key="ii">
								<u-td>{{vv.time}}</u-td>
								<u-td>{{vv.shi_cheng}}</u-td>
								<u-td>{{vv.chong}}</u-td>
								<u-td>{{vv.sha}}</u-td>
							</u-tr>
						</u-table>
					</block>
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
				api_name: '吉时查询',
				api_url: app.globalData.api_url,
				sui: 18,
				sex: 0,
				shichen: '',
				showShiChen: false,
				shichenList: {
					year: false,
					month: true,
					day: false,
					hour: false,
					minute: false,
					second: false,
					timestamp: true
				},
				labelText: '',
				labelValue: '',
				labelShow: false,
				labelList: [{
						label: '祭祀：指祭拜祖先和神明等。',
						value: '祭祀'
					},
					{
						label: '安葬：举行埋葬等仪式。',
						value: '安葬'
					},
					{
						label: '嫁娶：结婚的日子。',
						value: '嫁娶'
					},
					{
						label: '出行：外出旅游等。',
						value: '出行'
					},
					{
						label: '祈福：祈求神明降福或设醮还愿之事。',
						value: '祈福'
					},
					{
						label: '动土：建筑房屋时、第一次动起锄头挖土。',
						value: '动土'
					},
					{
						label: '安床：指安置床铺。',
						value: '安床'
					},
					{
						label: '开光：佛像塑成后、供奉上位之事。',
						value: '开光'
					},
					{
						label: '纳采：订婚时受授聘金。',
						value: '纳采'
					},
					{
						label: '入殓：将尸体放入棺材。',
						value: '入殓'
					},
					{
						label: '移徙：搬家、迁移住所。',
						value: '移徙'
					},
					{
						label: '破土：指埋葬死人。',
						value: '破土'
					},
					{
						label: '解除：打扫房屋。',
						value: '解除'
					},
					{
						label: '入宅：搬入新家。',
						value: '入宅'
					},
					{
						label: '修造：修理建造房屋。',
						value: '修造'
					},
					{
						label: '栽种：种植作物。',
						value: '栽种'
					},
					{
						label: '开市：商店开张营业。',
						value: '开市'
					},
					{
						label: '移柩：移动棺材。',
						value: '移柩'
					},
					{
						label: '订盟：订婚仪式的一种，俗称小聘。',
						value: '订盟'
					},
					{
						label: '拆卸：拆毁房屋。',
						value: '拆卸'
					},
					{
						label: '立卷：订立各种契约互相买卖之事。',
						value: '立卷'
					},
					{
						label: '交易：订立各种契约互相买卖之事。',
						value: '交易'
					},
					{
						label: '求嗣：向神明祈求后代。',
						value: '求嗣'
					},
					{
						label: '上梁：给房屋装上大梁。',
						value: '上梁'
					},
					{
						label: '纳财：购屋产业、进货、收帐、收租、讨债、贷款、五谷入仓等。',
						value: '纳财'
					},
					{
						label: '起基：建筑时、第一次动起锄头挖土。',
						value: '起基'
					},
					{
						label: '斋醮：庙宇建醮前需举行的斋戒仪式。',
						value: '斋醮'
					},
					{
						label: '赴任：走马上任。',
						value: '赴任'
					},
					{
						label: '冠笄：男女年满二十岁所举行的成年礼仪式。',
						value: '冠笄'
					},
					{
						label: '安门：放置正门门框。',
						value: '安门'
					},
					{
						label: '修坟：修理坟墓。',
						value: '修坟'
					},
					{
						label: '挂匾：指悬挂招牌或各种匾额。',
						value: '挂匾'
					}
				],
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
					that.showMsg('请选择日期范围');
					return false;
				}
				if (!that.labelValue) {
					that.showMsg('请选择相关术语');
					return false;
				}
				//开始查询
				that.post('sm_cx/ji_ri', {
					date: that.shichen,
					something: that.labelValue
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen(e) {
				this.shichen = e.startDate + ',' + e.endDate;
			},
			changelabel(e) {
				this.labelText = e[0].label;
				this.labelValue = e[0].value;
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
