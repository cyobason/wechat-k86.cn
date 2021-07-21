<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">商标：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="regno" placeholder="请输入申请号"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">分类：</view>
						<view class="soInputValue">
							<u-input v-model="leiLabel" placeholder="请选择商标分类" type="select" :border="true"
								@click="leiShow = true" />
							<u-select v-model="leiShow" :list="leiList" @confirm="changeLei"></u-select>
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
				<image :src="'https://imgcdn.zuigao.com/' + list.reg_no + '.jpg'" mode="widthFix"></image>
				<view class="soListTitle l-b-t-primary">商标名</view>
				<view class="soListContent c-info">{{list.name}}</view>
				<view class="soListTitle l-b-t-primary">申请号</view>
				<view class="soListContent c-info">{{list.reg_no}}（申请日期：{{list.reg_date}}）</view>
				<view class="soListTitle l-b-t-primary">商标状态</view>
				<view class="soListContent">
					<u-tag :text="list.status" size="mini" mode="light" type="info" v-if="list.status == '未下证'" />
					<u-tag :text="list.status" size="mini" mode="light" type="success" v-if="list.status == '已注册'" />
					<u-tag :text="list.status" size="mini" mode="light" type="error" v-if="list.status == '未续展'" />
					<u-tag :text="list.status" size="mini" mode="light" type="primary" v-if="list.status == '注册公告'" />
					<u-tag :text="list.status" size="mini" mode="light" type="primary" v-if="list.status == '初审公告'" />
				</view>
				<view class="soListTitle l-b-t-primary">申请人</view>
				<view class="soListContent c-info">{{list.shen_qing_ren}}</view>
				<view class="soListContent c-info">{{list.address}}</view>
				<view class="soListTitle l-b-t-primary">初审公告期号</view>
				<view class="soListContent c-info">
					{{list.first_no.split(',')[0] ? list.first_no.split(',')[0] : '-'}}{{list.first_no.split(',')[0] ? '（初审公告日期：' + list.first_no.split(',')[1] + '）' : ''}}
				</view>
				<view class="soListTitle l-b-t-primary">注册公告期号</view>
				<view class="soListContent c-info">
					{{list.last_no.split(',')[0] ? list.last_no.split(',')[0] : '-'}}{{list.last_no.split(',')[0] ? '（注册公告日期：' + list.last_no.split(',')[1] + '）' : ''}}
				</view>
				<view class="soListTitle l-b-t-primary">专用权期限</view>
				<view class="soListContent c-info">
					{{list.last_no.split(',')[0] ? list.last_no.split(',')[1] : ''}} ~ {{list.use_end}}
				</view>
				<view class="soListTitle l-b-t-primary">代理人</view>
				<view class="soListContent c-info">{{list.agent.agent_name}}</view>
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
				api_name: '商标详情',
				api_url: app.globalData.api_url,
				showWin: false,
				regno: '',
				lei: 0,
				leiLabel: '',
				leiShow: false,
				leiList: [{
						label: '01.化学原料',
						value: 1
					},
					{
						label: '02.颜料油漆',
						value: 2
					},
					{
						label: '03.日化用品',
						value: 3
					},
					{
						label: '04.燃料油脂',
						value: 4
					},
					{
						label: '05.医药卫生',
						value: 5
					},
					{
						label: '06.五金金属',
						value: 6
					},
					{
						label: '07.机械设备',
						value: 7
					},
					{
						label: '08.手工器械',
						value: 8
					},
					{
						label: '09.科学仪器',
						value: 9
					},
					{
						label: '10.医疗器械',
						value: 10
					},
					{
						label: '11.家用电器',
						value: 11
					},
					{
						label: '12.运输工具',
						value: 12
					},
					{
						label: '13.军用烟花',
						value: 13
					},
					{
						label: '14.珠宝钟表',
						value: 14
					},
					{
						label: '15.乐器',
						value: 15
					},
					{
						label: '16.文化用品',
						value: 16
					},
					{
						label: '17.橡胶制品',
						value: 17
					},
					{
						label: '18.皮革皮具',
						value: 18
					},
					{
						label: '19.建筑材料',
						value: 19
					},
					{
						label: '20.家具',
						value: 20
					},
					{
						label: '21.家用器具',
						value: 21
					},
					{
						label: '22.绳网袋篷',
						value: 22
					},
					{
						label: '23.纺织纱线',
						value: 23
					},
					{
						label: '24.床单布料',
						value: 24
					},
					{
						label: '25.服装鞋帽',
						value: 25
					},
					{
						label: '26.花边拉链',
						value: 26
					},
					{
						label: '27.地毯席垫',
						value: 27
					},
					{
						label: '28.体育玩具',
						value: 28
					},
					{
						label: '29.食品罐头',
						value: 29
					},
					{
						label: '30.调味茶糖',
						value: 30
					},
					{
						label: '31.水果花木',
						value: 31
					},
					{
						label: '32.啤酒饮料',
						value: 32
					},
					{
						label: '33.酒',
						value: 33
					},
					{
						label: '34.烟草烟具',
						value: 34
					},
					{
						label: '35.广告贸易',
						value: 35
					},
					{
						label: '36.金融物管',
						value: 36
					},
					{
						label: '37.建筑修理',
						value: 37
					},
					{
						label: '38.通讯电信',
						value: 38
					},
					{
						label: '39.运输旅行',
						value: 39
					},
					{
						label: '40.材料处理',
						value: 40
					},
					{
						label: '41.教育娱乐',
						value: 41
					},
					{
						label: '42.科研服务',
						value: 42
					},
					{
						label: '43.餐饮酒店',
						value: 43
					},
					{
						label: '44.医疗园艺',
						value: 44
					},
					{
						label: '45.社会法律',
						value: 45
					}
				],
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
				if (!that.regno) {
					that.showMsg('请输入申请号');
					return false;
				}
				if (!that.lei) {
					that.showMsg('请选择商标分类');
					return false;
				}
				//开始查询
				that.post('/shang_biao/xiang_qing', {
					reg_no: that.regno,
					cls: that.lei,
				}, res => {
					that.showWin = true;
					that.list = res.data;
				});
			},
			changeLei(e) {
				this.lei = e[0].value;
				this.leiLabel = e[0].label;
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
