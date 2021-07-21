<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">商标：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="word" placeholder="商标名/申请号/申请人名称"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">类型：</view>
						<view class="soInputValue">
							<u-input v-model="leixingLabel" placeholder="请选择查询商标类型" type="select" :border="true"
								@click="leixingShow = true" />
							<u-select v-model="leixingShow" :list="leixingList" @confirm="changeLeiXing"></u-select>
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
				<view class="soListTitle">
					<text>商标名：</text>
					<text class="c-primary">{{v.name}}</text>
				</view>
				<view class="soListTitle">
					<text>申请人：</text>
					<text class="c-info">{{v.contact}}</text>
				</view>
				<u-row gutter="0">
					<u-col span="6">
						<view class="soListTitle">
							<text>分类：</text>
							<text class="c-info">{{v.cls}}类</text>
						</view>
					</u-col>
					<u-col span="6">
						<view class="soListTitle">
							<text>商标状态：</text>
							<text class="c-info" v-if="v.status == '未下证'">{{v.status}}</text>
							<text class="c-success" v-if="v.status == '已注册'">{{v.status}}</text>
							<text class="c-error" v-if="v.status == '未续展'">{{v.status}}</text>
							<text class="c-primary" v-if="v.status == '注册公告'">{{v.status}}</text>
							<text class="c-primary" v-if="v.status == '初审公告'">{{v.status}}</text>
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
						<view class="soListTitle">
							<text>申请日期：</text>
							<text class="c-info">{{v.reg_date}}</text>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="0">
					<u-col span="6">
						<view class="soListTitle">
							<text>初审公告：</text>
							<text class="c-info">{{v.first.split(',')[1] ? v.first.split(',')[1] : '-'}}</text>
						</view>
					</u-col>
					<u-col span="6">
						<view class="soListTitle">
							<text>注册公告：</text>
							<text class="c-info">{{v.last.split(',')[1] ? v.last.split(',')[1] : '-'}}</text>
						</view>
					</u-col>
				</u-row>
				<view class="soListTitle">
					<text>代理公司：</text>
					<text class="c-info">{{v.agent}}</text>
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
				api_name: '商标查询',
				api_url: app.globalData.api_url,
				word: '',
				leixing: 0,
				leixingLabel: '商标名',
				leixingShow: false,
				leixingList: [{
						label: '商标名',
						value: 0
					},
					{
						label: '申请号',
						value: 1
					},
					{
						label: '申请人',
						value: 2
					},
					{
						label: '商标拼音',
						value: 3
					},
					{
						label: '代理机构',
						value: 4
					}
				],
				page: 1,
				lei: 0,
				leiLabel: '全部分类',
				leiShow: false,
				leiList: [{
						label: '全部分类',
						value: 0
					},
					{
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
				top: 0,
				winH: app.globalData.winH - 169,
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
				if (!that.word) {
					that.showMsg('请输入商标名/申请号/申请人名称');
					return false;
				}
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/shang_biao/cha_xun', {
					lei_xing: that.leixing,
					word: that.word,
					lei_bie: that.lei,
					page: that.page
				}, res => {
					that.list = res.data;
				});
			},
			changeLeiXing(e) {
				this.leixing = e[0].value;
				this.leixingLabel = e[0].label;
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
